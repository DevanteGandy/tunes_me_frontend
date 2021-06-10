<template lang="html" class="primary_bg">
  <div class="primary_bg">
    <v-container fluid class="mb-15">
      <v-layout row>
        <v-flex>
          <img src="../assets/music.svg" width="95%" />
        </v-flex>
        <v-flex md6 lg6 sm12 xs12 x6 pl-3>
          <h1
            class="white--text font-weight-medium mt-16 d-sm-pl-8 d-xs-pl-6"
          ></h1>
          <h2 class="black--text d-sm-pl-6">
            Music is <span class="yellow--text">everything </span>
          </h2>
          <v-dialog v-model="dialog" scrollable max-width="300px">
            <template v-slot:activator="{ on, attrs }">
              <v-btn color="black--text white" dark v-bind="attrs" v-on="on">
                Login
              </v-btn>
            </template>
            <v-card>
              <v-card-title>Login</v-card-title>
              <v-divider></v-divider>
              <v-card-text class="mt-2">
                <v-text-field
                  v-model="email"
                  solo
                  label="Email"
                  prepend-inner-icon="mdi-email"
                ></v-text-field>

                <v-text-field
                  v-model="password"
                  :type="show1 ? 'text' : 'password'"
                  name="input-10-1"
                  @click:append="show1 = !show1"
                  solo
                  :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
                  label="Password"
                  prepend-inner-icon="mdi-account-key"
                ></v-text-field>
              </v-card-text>
              <v-divider></v-divider>
              <v-card-actions>
                <v-btn color="blue darken-1" text @click="dialog = false"
                  >Close</v-btn
                >
                <v-btn color="blue darken-1" text @click.prevent="authenticate"
                  >login</v-btn
                >
              </v-card-actions>
            </v-card>
          </v-dialog>
          <v-btn class="ml-2" color="black--text white" dark>
            <router-link to="/signup" tag="span">Create account</router-link>
          </v-btn>
        </v-flex>
      </v-layout>
    </v-container>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      show1: false,
      email: "",
      password: "",
      dialog: false,
    };
  },
  methods: {
    authenticate() {
      const payload = {
        email: this.email,
        password: this.password,
      };
      axios
        .post(this.$store.state.endpoints.obtainJWT, payload)
        .then((response) => {
          this.$store.commit("updateToken", response.data.token);

          const base = {
            baseURL: this.$store.state.endpoints.baseUrl,
            headers: {
              Authorization: `JWT ${this.$store.state.jwt}`,
              "Content-Type": "application/json",
            },
            xhrFields: {
              withCredentials: true,
            },
          };
          const axiosInstance = axios.create(base);
          axiosInstance({
            url: "/rest-auth/user/",
            method: "get",
            params: {},
          }).then((response) => {
            this.$store.commit("setAuthUser", {
              authUser: response.data,
              isAuthenticated: true,
            });
            this.$router.push({ name: "Songs" });
          });
        })
        .catch((error) => {
          console.log(error);
          console.debug(error);
          console.dir(error);
        });
    },
  },
};
</script>

<style lang="css" scoped></style>
