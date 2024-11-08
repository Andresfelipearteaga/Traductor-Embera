<template>
    <v-app>
      <v-app-bar app color="white" dense height="70">
        <img
        src="../assets/TraductorEMBERA.png"
        height="60"
        contain
      >
      <v-spacer></v-spacer>
      <v-btn icon @click="openUserMenu">
        <v-icon>mdi-home </v-icon>
      </v-btn>
      <v-icon @click="remover">mdi-close</v-icon>

    </v-app-bar>
      <v-container class="background">
        <v-row>
          <!-- Formulario para agregar palabras -->
          <v-col cols="12" sm="6" md="4">
            <v-card>
              <v-card-title class="orange darken-3">
                <h3 class="white--text">Introducir palabras</h3>
              </v-card-title>
              <v-card-text>
                <v-form @submit.prevent="agregarPalabra">
                  <v-text-field v-model="espanol" label="Agregue palabra en español" required :error="error"></v-text-field>
                  <v-text-field v-model="embera" label="Agregue palabra en embera" required :error="error"></v-text-field>
                  <v-btn type="submit" color="orange darken-3 white--text">Guardar</v-btn>
                </v-form>
                <br>
                <div v-if="message" class="mensaje">{{ message }}</div>

              </v-card-text>
            </v-card>
          </v-col>
  
          <!-- Tabla para mostrar palabras -->
          <v-col cols="12" sm="6" md="8">
            <v-card>
              <v-card-title class="orange darken-3">
                <h3 class="white--text">Palabras Registradas</h3>
              </v-card-title>
              <v-card-text>
                <v-data-table :headers="headers" :items="palabras" item-key="id" class="elevation-1">
                <template v-slot:items="props">
                  <tr v-for="(palabra, index) in props.item" :key="index">
                    <td>{{ palabra }}</td>
                  </tr>
                </template>
              </v-data-table>
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-app>
  </template>
  
  <script>
import axios from 'axios'

export default {
    data() {
      return {
        espanol: "",
        embera: "",
        palabras: [], 
        headers: [
          { text: "Español", value: 'espanol'},
          { text: "Embera", value: 'embera' },
        ],
        error: false, 
        message: ""

      };
    },
    created() {
    // Verificar si hay una sesión activa
    const isLoggedIn = localStorage.getItem('isLoggedIn');

    if (!isLoggedIn) {
      // No hay sesión activa, redirigir a la página de inicio de sesión
      this.$router.push('/login');
    }
    this.obtenerPalabras();

  },
    methods: {
      remover(){
  localStorage.removeItem('isLoggedIn')
  this.$router.push('/login');

      },
      openUserMenu(){
      this.$router.push('/');
    },
      agregarPalabra() {
        if (!this.espanol || !this.embera) {
        this.error = true; 
        return; 
      }
        const paquete = 
        {
      espanol: this.espanol,
      embera: this.embera
    }
        axios.post(`https://edutlasdeveloper.pythonanywhere.com/palabras`, { paquete: paquete }) 
        .then(response => {
          this.message = response.data.msge
          this.obtenerPalabras()
          setTimeout(() => {
            this.message = "";
          }, 3000
          );
          this.espanol = ""
          this.embera = ""
          this.error = false; 

        })
        .catch(error => {
          console.error(error);
        });
      },
      obtenerPalabras() {
        axios.get(`https://edutlasdeveloper.pythonanywhere.com/obtener`) 
        .then(response => {
          this.palabras = response.data || [];
          console.log(this.palabras)
        })
        .catch(error => {
          console.error(error);
        });
      },
    },

  };
  </script>
  
  <style scoped>
.background {
  background-image: url('../assets/3.jpg'); /* Reemplaza 'ruta/de/tu/imagen.jpg' con la ruta de tu imagen de fondo */
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  min-height: 100vh; /* Ajusta esto según sea necesario */

.v-text-field.error {
  border-color: red;
}

.mensaje {
  background-color: green;
  color: white;
  padding: 10px;
  border-radius: 20px;

}

}  </style>
  