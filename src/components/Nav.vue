<template>
  <v-toolbar elevation="2" class="nav px-16 grey lighten-3">
    <v-btn icon to="/">
      <v-img class="logo" src="@/assets/gathogar_logo.png"></v-img>
    </v-btn>
    <v-spacer></v-spacer>
    <div class="hidden-sm-and-down">
      <v-btn text v-if="logueado" to="/misPublicaciones" class="px-0">Mis publicaciones</v-btn>
      <v-btn text to="/publicar">Publicar</v-btn>
      <v-btn
        v-if="!logueado"
        elevation="2"
        outlined
        color="success"
        class=""
        @click.stop="dialog = true"
        >Ingresar</v-btn
      >
      <v-btn
        v-if="logueado"
        elevation="2"
        outlined
        color="warning"
        class="ml-5"
        @click="logOut"
        >salir</v-btn
      >
    </div>
    <v-menu nudge-left="100">
      <template v-slot:activator="{ on, attrs }">
        <v-btn icon class="hidden-md-and-up" v-bind="attrs" v-on="on"
          ><v-icon class="">mdi-dots-vertical</v-icon>
        </v-btn>
      </template>
      <v-list class="mt-8 text-left px-5" min-width="100" dark>
        <v-list-item-title
          ><v-btn v-if="logueado" to="/misPublicaciones" class="mb-3" color="succes" outlined
            >Mis publicaciones</v-btn
          ></v-list-item-title
        >
        <v-list-item-title
          ><v-btn to="/publicar" color="succes" outlined
            >Publicar</v-btn
          ></v-list-item-title
        >
        <v-list-item-title class="my-3"
          ><v-btn v-if="!logueado" @click.stop="dialog = true" color="succes" outlined
            >Ingresar</v-btn
          ></v-list-item-title
        >
        <v-list-item-title
          ><v-btn v-if="logueado" color="succes" outlined @click="logOut"
            >Salir</v-btn
          ></v-list-item-title
        >
      </v-list>
    </v-menu>
    <v-dialog
        v-model="dialog"
        transition="dialog-top-transition"
        max-width="500"
      >
        <v-card>
          <v-toolbar color="success" dark>Ingresa</v-toolbar>
          <v-card-text class="mt-5">
            <label>E-Mail</label>
            <v-text-field
              label="E-mail"
              v-model="registro.email"
              outlined
              dense
              required
            ></v-text-field>
            <label>Contraseña</label>
            <v-text-field
              v-model="registro.contraseña"
              label="Contraseña"
              type="password"
              outlined
              dense
              required
            ></v-text-field>
          </v-card-text>
          <v-card-actions class="justify-center">
            <v-btn @click="login" color="success">Ingresar</v-btn>
            <v-btn to="/registrar" @click="dialog.value = false" color="warning"
              >Regístrate</v-btn
            >
            <v-btn text @click="dialog.value = false">Cerrar</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
  </v-toolbar>
</template>

<script>
import { mapState, mapActions, mapMutations } from "vuex";
import firebase from "firebase";
export default {
  name: "Nav",
  data() {
    return {
      dialog: false,
    };
  },
  methods: {
    ...mapActions(["logOut"]),
    ...mapMutations(["setLogin"]),
    async login() {
      if (!this.registro && !this.registro.email && !this.registro.contraseña)
        return;

      try {
        const db = firebase.firestore();
        const reqDBUser = await db.collection("usuarios").get();
        reqDBUser.docs.find((el) => this.registro.email === el.data().email);
        const req = await firebase
          .auth()
          .signInWithEmailAndPassword(
            this.registro.email,
            this.registro.contraseña
          );
        if (req && req !== null) {
          localStorage.setItem("login", "logueado");
          this.setLogin(true);
          this.dialog = false;
        }
      } catch (error) {
        const errorCode = error.code;
        const errorMsje = error.message;
        console.log("Codigo de error:", errorCode);
        console.log("Mensaje de error", errorMsje);
      }
    },
  },
  computed: {
    ...mapState(["registro"]),
    navLocalStorage() {
      const logVar = localStorage.getItem("login");
      return logVar;
    },
    ...mapState(["logueado"]),
  },
};
</script>

<style lang="scss" scoped>
@import url("https://fonts.googleapis.com/css2?family=Playfair+Display:wght@600&display=swap");
@import "@/scss/main.scss";

.nav {
  width: 100%;
  z-index: 1;
  position: fixed;
  font-family: $main-font;
}

.logo {
  width: 3.5rem;
}
.title {
  font-family: "Playfair Display", serif !important;
}
.title {
  visibility: hidden;
  font-family: "Playfair Display", serif !important;
}
@media (min-width: 480px) {
  .title {
    visibility: visible;
  }
}
</style>
