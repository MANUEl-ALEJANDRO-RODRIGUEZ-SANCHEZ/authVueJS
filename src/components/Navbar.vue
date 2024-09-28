<template>
  <nav class="navbar navbar-expand-lg navbar-light bg-light">
    <div class="container-fluid">
      <a class="navbar-brand" href="#"><img src="../assets/logo.png"/></a>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav me-auto mb-2 mb-lg-0">
          <li class="nav-item">
            <router-link class="nav-link active" aria-current="page" to="/">Home</router-link>
          </li>
          <li class="nav-item" v-if="existeUsuario">
            <router-link class="nav-link active" aria-current="page" to="/about">About</router-link>
          </li>
          <li class="nav-item" v-if="existeUsuario">
            <router-link class="nav-link active" aria-current="page" to="/contact">Contact</router-link>
          </li>
        </ul>
        <!-- Botón de Cerrar sesión -->
        <button v-if="existeUsuario" class="btn btn-outline-danger me-2" @click="signout">
          Log out
        </button>
      </div>
    </div>
  </nav>
</template>

<script>
import { getAuth, signOut } from "firebase/auth";
import { mapGetters } from 'vuex';

export default {
  name: 'Navbar',
  methods: {
    signout () {
      const auth = getAuth();
      signOut(auth).then(() => {
        alert('¡Sesión cerrada! Inicia sesión.');
      }).catch((error) => {
        console.error("Error al cerrar sesión: ", error);
      });
    }
  },
  computed: {
    ...mapGetters(['existeUsuario'])
  }
}
</script>

<style>
  img{
    width: 32px;
    height: auto;
  }
</style>