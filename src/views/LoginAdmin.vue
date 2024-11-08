<template>
    <v-app id="inspire">
      <v-app-bar app color="white" dense height="70">
        <img
        src="../assets/TraductorEMBERA.png"
        height="60"
        contain
      >
      <v-spacer></v-spacer>
      <v-btn icon @click="openUserMenu">
        <v-icon>mdi-home  </v-icon>
      </v-btn>
    </v-app-bar>
      <div class="background">
        <v-content>
        <v-container class="fill-height" fluid >
          <v-row align="center" justify="center">
            <v-col cols="12" sm="8" md="8">
              <v-card class="elevation-12">
                <v-window v-model="step" style="padding: 10%;">
                  <v-window-item :value="1">
                    <v-row>
                      <v-col cols="12" md="8">
                        <v-card-text class="mt-12">
                          <h2
                            class="text-center orange--text text--darken-3"
                
                          >Ingresar</h2>
  
                          <v-form>
                            <span class="mdi mdi-account"></span>
                            <v-text-field
                              label="Usuario"
                              name="Usuario"
                              type="text"
                              color="orange darken-3"
                              v-model="username"
                            />
                            <span class="mdi mdi-lock"></span>
                            <v-text-field
                              id="password"
                              label="Contraseña"
                              name="Contraseña"
                              :type="showPassword ? 'text' : 'password'"
                              color="orange darken-3"
                              v-model="password"
                              append-icon="mdi-eye"
                              @click:append="togglePasswordVisibility"

                            />
                          </v-form>
                        </v-card-text>
                        <div class="text-center mt-3">
                          <v-btn rounded color="orange darken-1" dark @click="signIn">Iniciar Sesión</v-btn>
                        </div>
                        <br>
                        <div v-if="msge" class="mensaje">{{ msge }}</div>

                      </v-col>
                      <v-col cols="12" md="4" class="custom-background rounded-lg">
                        <v-card-text class="brown--text text--darken-3 mt-16">
                          <h1 class="text-center display-1 transparent--text">Bienvenido</h1>
                          <h3
                            class="text-center transparent--text"
                          >Inicia sesión para poder administrar la base de datos</h3>
                        </v-card-text>
                      </v-col>
                    </v-row>
                  </v-window-item>
                </v-window>
              </v-card>
            </v-col>
          </v-row>
        </v-container>
      </v-content>
      </div>
    </v-app>
  </template>
  
  <script>

  export default {
    data: () => ({
      step: 1,
      username: "",
      password: "", 
      showPassword: false,
      msge: ""

      
    }),
    props: {
      source: String
    },
    methods: {
    signIn() {
      // Verificar si las credenciales son correctas
      if (this.username === "admin" && this.password === "admin123") {
        localStorage.setItem('isLoggedIn', 'true')
        this.$router.push('/panelConfiguracion');
        this.obtenerPalabras()
      } else {
        // Lógica para manejar credenciales incorrectas (puedes mostrar un mensaje de error, etc.)
        this.msge = "Credenciales incorrectas";
        setTimeout(() => {
            this.msge = "";
          }, 2000
          );
      }
    },
    openUserMenu(){
      this.$router.push('/');
    },
    togglePasswordVisibility() {
      this.showPassword = !this.showPassword;
    }
    
  },
  };
  </script>

<style scoped>

@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@400;600;700&family=Raleway:wght@600&family=Roboto:wght@300;900&family=Source+Code+Pro:wght@700&display=swap');
.background {
background-image: url('../assets/indigena2.jpg');
background-size: cover;
height: 100vh;
}

* {
    font-family: 'Source code pro', monospace;
  }

  h2 {
    font-family: 'Source code pro', monospace;
    font-size: 25pt;
  }


.custom-background {
  background: url('../assets/6.png') center center/cover no-repeat; /* Ajusta la ruta de la imagen */
}

@media only screen and (max-width: 768px) {
  .custom-background {
    background: url('../assets/5.png') center center/cover no-repeat;
  }

}

.mensaje {
  background-color: rgb(255, 51, 51);
  color: white;
  padding: 10px;
  border-radius: 20px;
  text-align: center;

}
</style>
  