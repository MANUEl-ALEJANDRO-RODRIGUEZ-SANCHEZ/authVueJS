<template>
  <div class="container">
    <Navbar/>
  </div>
  <div class="auth-container">
    <!-- Verifica si el usuario está autenticado -->
    <div v-if="!existeUsuario" class="card auth-card">
      <div class="card-body">
        <h3 class="card-title text-center">{{ isLogin ? 'Iniciar sesión' : 'Registrarse' }}</h3>
        
        <form @submit.prevent="isLogin ? login() : register()">
          <!-- Email -->
          <div class="mb-3">
            <label for="email" class="form-label">Correo electrónico</label>
            <input type="email" v-model="email" class="form-control" required>
          </div>

          <!-- Password -->
          <div class="mb-3">
            <label for="password" class="form-label">Contraseña</label>
            <input type="password" v-model="password" class="form-control" required>
          </div>

          <!-- Repetir password solo si es registro -->
          <div v-if="!isLogin" class="mb-3">
            <label for="repassword" class="form-label">Repetir contraseña</label>
            <input type="password" v-model="repassword" class="form-control" required>
          </div>

          <button type="submit" class="btn btn-primary w-100">{{ isLogin ? 'Iniciar sesión' : 'Registrar' }}</button>
        </form>

        <div class="text-center mt-3">
          <p>{{ isLogin ? '¿No tienes cuenta?' : '¿Ya tienes cuenta?' }}
            <a href="#" @click.prevent="toggleForm">{{ isLogin ? 'Regístrate' : 'Iniciar sesión' }}</a>
          </p>
        </div>
      </div>
    </div>
    
    <!-- Mensaje de bienvenida si el usuario está autenticado -->
    <div v-else>
      <h3 class="text-center">¡Bienvenido!, sesión iniciada</h3>
    </div>
  </div>
</template>

<script>
import Navbar from '@/components/Navbar.vue'
import { getAuth, signInWithEmailAndPassword, createUserWithEmailAndPassword } from 'firebase/auth';
import { mapGetters } from 'vuex';

export default {
  name: 'Home',
  components: {
    Navbar
  },
  data() {
    return {
      isLogin: true,
      email: '',
      password: '',
      repassword: '',
      errorMessage: ''
    };
  },
  computed: {
    ...mapGetters(['existeUsuario'])
  },
  methods: {
    toggleForm() {
      this.isLogin = !this.isLogin;
    },
    login() {
      const auth = getAuth();
      signInWithEmailAndPassword(auth, this.email, this.password)
        .then(() => {
          alert('¡Sesión iniciada!');
        })
        .catch((error) => {
          this.errorMessage = error.message;
          alert(this.errorMessage);
        });
    },
    register() {
      if (this.password !== this.repassword) {
        alert("Las contraseñas no coinciden.");
        return;
      }
      const auth = getAuth();
      createUserWithEmailAndPassword(auth, this.email, this.password)
        .then(() => {
          alert('¡Registrado!');
        })
        .catch((error) => {
          this.errorMessage = error.message;
          alert(this.errorMessage);
        });
    }
  }
}
</script>

<style scoped>
.auth-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #f8f9fa;
}

.auth-card {
  width: 100%;
  max-width: 400px;
}

.auth-card .card-body {
  padding: 2rem;
}
</style>
