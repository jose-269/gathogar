<template>
  <div class="bg-anuncios">
    <v-container class="py-16">
      <v-row>
        <v-col
          xs="12"
          sm="6"
          md="4"
          lg="3"
          xl="3"
          v-for="(gatito, i) in gatosDB"
          :key="i"
        >
          <v-dialog max-width="350" :retain-focus="false">
            <template v-slot:activator="{ on, attrs }">
              <v-card
                class="cards"
                height="100%"
                @click="dialog = true"
                v-bind="attrs"
                v-on="on"
              >
                <v-card-title
                  class="
                    subtitle-1
                    font-weight-bold
                    card-title
                    grey
                    darken-4
                    white--text
                    justify-center
                    d-flex
                    mb-3
                  "
                >
                  {{ gatito.nombre }}</v-card-title
                >
                <div class="mx-auto mb-8">
                  <v-img
                    v-if="gatito.img"
                    height="300"
                    :src="gatito.img"
                    contain
                    class="mb-3 image__item"
                  ></v-img>
                  <v-img
                    v-else
                    height="300"
                    class="mb-3"
                    src="../assets/gathogar_logo.png"
                  ></v-img>
                  <v-divider></v-divider>
                  <v-list-item>
                    <v-list-item-content>
                      <v-list-item-title class="card-text"
                        >Cantidad: {{ gatito.cantidad }}</v-list-item-title
                      >
                      <v-list-item-title class="card-text"
                        >Region: {{ gatito.region }}</v-list-item-title
                      >
                    </v-list-item-content>
                  </v-list-item>
                  <v-card-actions>
                    <div class="div">
                      <v-btn
                        class="btn"
                        v-if="logueado"
                        @click="contacto(gatito.telefono)"
                        color="cyan darken-3"
                        >Te gustaría adoptarme?</v-btn
                      >
                    </div>
                  </v-card-actions>
                </div>
              </v-card>
            </template>
            <v-card height="100%">
              <v-card-title
                class="
                  subtitle-1
                  font-weight-bold
                  card-title
                  grey
                  darken-4
                  white--text
                  justify-center
                  d-flex
                  mb-3
                "
              >
                {{ gatito.nombre }}</v-card-title
              >
              <v-img height="400" contain :src="gatito.img"></v-img>
              <v-divider></v-divider>
              <v-card-text class="pt-5 card-text font-weight-black">
                {{ gatito.mensaje }}
              </v-card-text>
            </v-card>
          </v-dialog>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
import { mapState } from "vuex";
export default {
  name: "Anuncios",
  data() {
    return {
      login: "",
      dialog: false,
    };
  },
  props: {
    gatos: {
      type: Array,
    },
    lugar: {
      type: Array,
    },
    gatosDB: {
      type: Array,
    },
  },
  methods: {
    contacto(telefono) {
      const respuesta = confirm(
        "¿Quieres ponerte en contacto con el dueño del gato?"
      );
      if (respuesta) {
        window.location = `https://api.whatsapp.com/send?phone=${telefono}`;
      }
    },
  },
  computed: {
    ...mapState(["logueado"]),
  },
};
</script>

<style lang="scss" scoped>
@import "@/scss/main.scss";
.bg-anuncios {
  // background-color: #8bffff;
  background-color: #eeeeee;
}
.card-text {
  font-family: $main-font;
}
.btn {
  position: absolute;
  left: 50%;
  margin-left: -125.414px;
  bottom: 3%;
}
.cards {
  box-shadow: 5px 10px #7a7a7a;
  transition: box-shadow 0.3s ease-in-out;
  cursor: pointer;
}
.cards:hover {
  box-shadow: 0 10px 20px #7a7a7a;
}
// @media (min-width: 320px) {
//   .btn {
//     left: 25px;
//   }
// }
// @media (min-width: 360px) {
//   .btn {
//     left: 50px;
//   }
// }
// @media (min-width: 375px) {
//   .btn {
//     left: 57px;
//   }
// }
// @media (min-width: 411px) {
//   .btn {
//     left: 75px;
//   }
// }
// @media (min-width: 480px) {
//   .btn {
//     left: 104px;
//   }
// }
// @media (min-width: 540px) {
//   .btn {
//     left: 135px;
//   }
// }
// @media (min-width: 540px) {
//   .btn {
//     left: 135px;
//   }
// }
// @media (min-width: 600px) {
//   .btn {
//     left: 17px;
//   }
// }
// @media (min-width: 768px) {
//   .btn {
//     left: 60px;
//   }
// }
// @media (min-width: 960px) {
//   .btn {
//     left: 18px;
//   }
// }
// @media (min-width: 992px) {
//   .btn {
//     left: 18px;
//   }
// }
</style>
>
