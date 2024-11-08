<template>
    <v-app>
    <!-- App Bar principal en blanco -->
    <v-app-bar app color="white" dense height="70">
        <img
        src="../assets/TraductorEMBERA.png"
        height="60"
        contain
      >
      <v-spacer></v-spacer>
      <v-btn icon @click="openUserMenu">
        <v-icon>mdi-account</v-icon>
      </v-btn>
    </v-app-bar>
    <!-- Segundo App Bar o Navbar azul -->
    <div :style="{ background: 'linear-gradient(to right, #FF6300, #FF9959)', height: '40px' }">
      <div  :style="{margin: '10px', color: 'white' }">
        <p><strong>Lenguas: </strong> Español, Embera</p>
        <div class="text-center" width="20px">
            <v-btn icon @click="intercambiarIdiomas">
        <v-icon>mdi-swap-horizontal</v-icon>
      </v-btn>
          </div>
      </div>
  </div>
      <v-main class="background">
        <v-container fluid >
          <div class="center">
            <v-row>
          <v-col cols="12" md="6">
            <v-row>
              <v-col cols="12" md="3" class="text-center">
                <v-select
                bg-color="amber-lighten-4"
                color="orange orange-darken-4"
                  v-model="selectedLanguage1"
                  :items="languages"
                  label="Idioma"
                ></v-select>
              </v-col>
            </v-row>
              <v-row> <v-col cols="12" md="10" class="text-center">
                <v-textarea
                bg-color="amber-lighten-4"
                color="orange orange-darken-4"
                  v-model="texto"
                  placeholder="Traduce aqui"
                  auto-grow
                  @input="reiniciarTemporizador"
                  class="textarea"
                  ></v-textarea>
              </v-col>
            </v-row>
          </v-col>

          
          <v-col cols="12" md="6">
            <v-row>
              <v-col cols="19" md="3" class="text-center">
                <v-select
                bg-color="amber-lighten-4"
                color="orange orange-darken-4"
                  v-model="selectedLanguage2"
                  :items="filteredLanguages"
                  label="Idioma"
                  class="custom-select"
                ></v-select>
              </v-col>
            </v-row>
            <v-row>
              <v-col cols="12" md="10" class="text-center">
                <v-textarea
                  bg-color="amber-lighten-4"
                  color="orange orange-darken-4"
                  v-model="translate"
                  placeholder="Traducción"
                  auto-grow
                  class="translatearea"
                  readonly
                  :style ="{cursor: 'pointer'}"
                ></v-textarea>
              </v-col>
            </v-row>
          </v-col>
        </v-row>

      
          </div>
          <v-row>
        <v-col>
          <!-- Espacios para Logos -->
          <v-row>
            <div class="logos">
            <v-col class="text-center mb-4">
                <img src="../assets/logoActualizado.jpg" alt="Logo" height="70px" />
            
                </v-col>
            <v-col class="text-center mb-4">
                <img src="../assets/lexicon.png" alt="Logo" height="70px" />
            </v-col>
          </div>


          </v-row>
        </v-col>
      </v-row>
        

      </v-container>
      </v-main>
    </v-app>
  </template>
  
  <script>
import axios from 'axios'
export default {
    data() {
      return {
        drawer: false,
        selectedLanguage1: "Español",
        selectedLanguage2: "Embera",
        languages: ["Español", "Embera"], 
        texto: "",
        translate: "",
        temporizador: null
      };
    },
    computed: {
    filteredLanguages() {
      return this.languages.filter(
        (lang) => lang !== this.selectedLanguage1
      );
    },
  },
  watch: {
    selectedLanguage1() {
      // Actualizar el segundo v-select al seleccionar un idioma en el primero
      this.selectedLanguage2 = this.filteredLanguages[0];
    },
  },
    methods: {
      openUserMenu() {
        const isLoggedIn = localStorage.getItem('isLoggedIn');

        if (isLoggedIn) {
        this.$router.push('/panelConfiguracion'); }
        else {
          this.$router.push('/login');
        }
    
},




      reiniciarTemporizador() {
        clearTimeout(this.temporizador);
        this.temporizador = setTimeout(() => {
          this.traducir();
        }, 1000)  
      },

      traducir() {
        const paquete = 
        {
      idiom1: this.selectedLanguage1,
      idiom2: this.selectedLanguage2,
      texto: this.texto,}

        axios.post(`https://edutlasdeveloper.pythonanywhere.com/translate`, {paquete: paquete}) 
        .then(response => {
          this.translate = response.data.translate
          console.log(this.translate)
        })
        .catch(error => {
          console.error(error);
        });
      },
      intercambiarIdiomas() {
      const temp = this.selectedLanguage1;
      this.selectedLanguage1 = this.selectedLanguage2;
      this.selectedLanguage2 = temp;

      const tempTexto = this.texto;
      this.texto = this.translate;
      this.translate = tempTexto;
    }
    },
  };
  </script>

  <style>

@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@400;600;700&family=Raleway:wght@600&family=Roboto:wght@300;900&family=Source+Code+Pro:wght@700&display=swap');


* {
    font-family: 'Source code pro', monospace;
    font-weight: 700;
  }

.textarea {
  background-color: rgb(255, 255, 255);
  border-radius: 20px;
  padding: 10px;
  border: 1px solid rgb(182, 182, 182);

}

.translatearea {
  background-color: rgb(233, 233, 233);
  border-radius: 20px;
  padding: 10px;

}

.textarea .v-input__control input:disabled {
  color: black; /* Color del texto cuando está deshabilitado */
}

@media (min-width: 951px) {
.center {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-left: 100px;
}
}

.background {
  background-image: url('../assets/3.jpg'); /* Reemplaza 'ruta/de/tu/imagen.jpg' con la ruta de tu imagen de fondo */
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  min-height: 100vh; /* Ajusta esto según sea necesario */
}


.logos {
  margin-top: 50px;
  background: rgb(255,255,255);
  background: linear-gradient(0deg, rgba(255,255,255,1) 70%, rgba(255,255,255,0) 100%);  display: flex;
  justify-content: center;
  align-items: center;
  width: 100vw;
  -webkit-box-shadow: 3px 17px 22px -13px rgb(255, 255, 255);
-moz-box-shadow: 3px 17px 22px -13px rgb(255, 255, 255);
box-shadow: 3px 17px 22px -13px rgb(255, 255, 255);
}
</style>
  