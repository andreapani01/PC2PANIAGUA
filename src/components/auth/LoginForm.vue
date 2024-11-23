<template>
    <q-layout view="lHh Lpr lFf">
        <br>
        <div class="text-center q-mb-xl">
            <div class="text-h4 text-primary">Practica 2</div>
        </div>
      <q-page-container class="flex flex-center full-height">
        
        <q-card style="width: 350px;">
            <q-card-section>
                <div class="text-h6">Inicio de sesión</div>
            </q-card-section>
          <q-card-section>
            <q-input
            v-model="email"
            label="Correo electrónico"
            type="email"
            outlined
            dense
            :rules="[val => !!val || 'El correo es obligatorio']"
          />
            <q-input
              v-model="password"
              label="Contraseña"
              :type="isPwd ? 'password' : 'text'"
              outlined
              dense
              hint="Password"
            >
              <template v-slot:append>
                <q-icon
                  :name="isPwd ? 'visibility_off' : 'visibility'"
                  class="cursor-pointer"
                  @click="isPwd = !isPwd"
                />
              </template>
            </q-input>
            <q-btn
            label="Iniciar sesión"
            type="submit"
            color="primary"
            unelevated
            class="full-width q-mt-md"
            @click="goToLogin"
          />
          <div class="text-center q-mt-md">
            <q-btn
              label="Registrarse"
              flat
              color="secondary"
            />
          </div>
          </q-card-section>
        </q-card>
      </q-page-container>
    </q-layout>
  </template>
  
  <style></style>
  
  <script>
  export default {
    name: "LoginForm",
    data() {
      return {
        email: "",
        password: "",
        isPwd: true,
      };
    },
    methods: {
      goToLogin() {
        console.log("Click en el boton de inici de sesión");
        console.log("Valor del email: " + this.email);
  
        let endpointLogin = "/api/v1/user/signin";
        let user = { email: this.email, password: this.password };
  
        this.$api
          .post(endpointLogin, user)
          .then((response) => {
            localStorage.setItem("userData", JSON.stringify(response));
            this.$q.notify({
              message: "Bienvenido a Practica 2",
              color: "positive",
              position: "bottom",
              timeout: 5000,
            });
  
            this.$router.push("/movies");
          })
          .catch((error) => {
            //Ocurrió un error
            this.$q.notify({
              message: "Ocurrió un error en las credenciales",
              color: "negative",
              position: "top",
              timeout: 10000,
            });
            console.log("Error en: " + error);
          });
      },
    },
  };
  </script>