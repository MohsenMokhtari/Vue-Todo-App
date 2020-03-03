<template>
  <v-content class="bg">
    <v-container fluid>
      <v-layout align-center justify-center>
        <v-flex xs4 class="pa-2" style="margin-top: 20vh;">
          <v-card class="pa-3" color="rgba(54, 54, 57, 0.4)">
            <v-layout column>
              <v-text-field
                color="#005778"
                prepend-inner-icon="account_circle"
                v-model="username"
                dark
                label="Username"
              ></v-text-field>
              <v-text-field
                color="#005778"
                :append-icon="show1 ? 'visibility_off' : 'visibility'"
                prepend-inner-icon="vpn_key"
                v-model="password"
                dark
                :type="show1 ? 'text' : 'password'"
                label="Password"
                @click:append="show1 = !show1"
                @keyup.enter="login"
              ></v-text-field>
              <v-layout justify-center column>
                <v-btn
                  color="#005778"
                  class="white--text"
                  :loading="isAuthLoading"
                  round
                  @click="login"
                >Login</v-btn>
                <v-dialog v-model="showModal" width="40vw">
                  <template v-slot:activator="{ on }">
                    <v-btn color="#005778" class="white--text" v-on="on" round>Register</v-btn>
                  </template>
                  <v-card>
                    <v-card-title
                      style="background-color: #005778;"
                      class="headline white--text"
                      primary-title
                    >Register</v-card-title>
                    <v-layout align-center justify-center>
                      <v-flex xs9>
                        <v-layout column class="pa-2">
                          <v-text-field
                            color="#005778"
                            prepend-inner-icon="account_circle"
                            v-model="registerUsername"
                            hint="Minimum 5 characters."
                            label="Username"
                          ></v-text-field>
                          <v-text-field
                            color="#005778"
                            prepend-inner-icon="mail"
                            v-model="registerMail"
                            required
                            label="Email Address"
                          ></v-text-field>
                          <v-text-field
                            color="#005778"
                            :append-icon="show2 ? 'visibility_off' : 'visibility'"
                            prepend-inner-icon="vpn_key"
                            v-model="registerPassword"
                            :type="show2 ? 'text' : 'password'"
                            hint="Minimum 8 characters."
                            label="Password"
                            @click:append="show2 = !show2"
                          ></v-text-field>
                          <v-text-field
                            color="#005778"
                            :append-icon="show3 ? 'visibility_off' : 'visibility'"
                            prepend-inner-icon="vpn_key"
                            v-model="registerPassword2"
                            :type="show3 ? 'text' : 'password'"
                            hint="Minimum 8 characters."
                            label="Confirm Password"
                            @click:append="show3 = !show3"
                          ></v-text-field>
                        </v-layout>
                      </v-flex>
                    </v-layout>

                    <v-card-actions>
                      <v-spacer></v-spacer>
                      <v-btn color="#005778" class="white--text mb-3" @click="register">Register</v-btn>
                      <v-spacer></v-spacer>
                    </v-card-actions>
                  </v-card>
                </v-dialog>
              </v-layout>
            </v-layout>
          </v-card>
        </v-flex>
      </v-layout>
    </v-container>
  </v-content>
</template>

<script>
/* import firebase from "firebase/app";
import "firebase/firestore"; */
import JQuery from "jquery";
let $ = JQuery;
import CryptoJS from "crypto-js";

export default {
  name: "Login",
  data() {
    return {
      username: "",
      email: "",
      password: "",
      showModal: false,
      isAuthLoading: false,
      show1: false,
      show2: false,
      show3: false,
      registerUsername: "",
      registerPassword: "",
      registerPassword2: "",
      registerMail: ""
    };
  },
  methods: {
    login() {
      var vm = this;
      this.isAuthLoading = true;

      $.ajax({
        type: "POST",
        url: "http://195.174.212.250:8088/methods/login.php",
        dataType: "JSON",
        data: {
          username: vm.username,
          password: vm.password
        },

        success: function(data) {
          switch (data) {
            case "userNotFound":
              alert("User not found");
              break;
            case "loginFailed":
              alert("Invalid login credentials");
              break;
            case "loginSuccessful":
              vm.$root.$emit("loginName", vm.username);
              vm.$router.push("/home");
              break;
          }
        },

        error: function(data) {
          alert("Something went wrong!");
          console.log(data);
        }
      });
    },
    show() {
      this.showModal = true;
    },
    register() {
      var vm = this;
      if (
        this.registerUsername &&
        this.registerPassword &&
        this.registerPassword2 &&
        this.registerMail
      ) {
        if (this.registerUsername.length >= 5) {
          if (
            this.registerPassword.length >= 8 &&
            this.registerPassword2.length >= 8
          ) {
            if (this.registerPassword === this.registerPassword2) {
              $.ajax({
                type: "POST",
                url: "http://http://195.174.212.250:8088/methods/register.php",
                dataType: "JSON",
                data: {
                  username: vm.registerUsername,
                  password: vm.registerPassword,
                  email: vm.registerMail
                },

                success: function(data) {
                  switch (data) {
                    case "OK":
                      vm.username = vm.registerUsername;
                      vm.password = vm.registerPassword;
                      vm.login();
                      break;
                  }
                },

                error: function(data) {
                  alert("Something went wrong!");
                  console.log(data);
                }
              });
            } else {
              alert("Passwords don't match.");
            }
          } else {
            alert("Password is shorter than 8 characters.");
          }
        } else {
          alert("Username is shorter than 5 characters.");
        }
      } else {
        alert("Fields can't be empty.");
      }
    }
  }
};
</script>

<style scoped>
.bg {
  background-image: linear-gradient(rgba(0, 0, 0, 0.6), rgba(0, 0, 0, 0.6)),
    url("../../bg.jpg");
  background-size: cover;
  background-position: center;
  height: 100vh;
  background-attachment: fixed;
}
</style>
