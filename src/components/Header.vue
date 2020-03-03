<template>
  <v-toolbar app fixed color="#005778">
    <v-toolbar-title class="headline white--text" @click="$router.replace('/home')">
      <v-icon medium color="white">check_circle</v-icon>
      <span class="font-weight-heavy ml-1">What To Do?</span>
    </v-toolbar-title>
    <v-spacer></v-spacer>

    <div class="text-xs-center" v-if="$route.path !== '/login'">
      <v-menu v-model="menu" :close-on-content-click="false" :nudge-width="200" offset-y>
        <template v-slot:activator="{ on }">
          <v-btn class="white--text" flat round v-on="on">My Profile</v-btn>
        </template>
        <v-card dark>
          <v-list>
            <v-list-tile avatar>
              <v-list-tile-avatar>
                <img
                  src="https://i0.wp.com/www.winhelponline.com/blog/wp-content/uploads/2017/12/user.png"
                  alt="John"
                >
              </v-list-tile-avatar>

              <v-list-tile-content>
                <v-list-tile-title>{{username}}</v-list-tile-title>
                <v-list-tile-sub-title>a user</v-list-tile-sub-title>
              </v-list-tile-content>
            </v-list-tile>
          </v-list>

          <v-divider></v-divider>

          <v-list>
            <v-list-tile v-for="(list, index) in lists" :key="list.id">
              <v-list-tile-action>
                <a class="list" @click="goToList(index)">{{list.name}}</a>
              </v-list-tile-action>
            </v-list-tile>
          </v-list>
        </v-card>
      </v-menu>
    </div>

    <v-tooltip bottom v-if="$route.path !== '/login'">
      <template v-slot:activator="{ on }">
        <v-btn flat round @click="signOut" v-on="on">
          <v-icon color="white">power_settings_new</v-icon>
        </v-btn>
      </template>
      <span>Sign Out</span>
    </v-tooltip>
  </v-toolbar>
</template>

<script>
import firebase from "firebase/app";
import "firebase/auth";
export default {
  name: "Header",
  data() {
    return {
      username: "",
      fav: true,
      menu: false,
      message: false,
      hints: true,
      lists: [
        {
          name: "Personal Work"
        },
        {
          name: "Company Stuff"
        }
      ]
    };
  },
  methods: {
    goToList(index) {
      this.$root.$emit("toList", index);
    },
    signOut() {
      var vm = this;
      vm.$router.replace("/login");
    }
  },
  mounted() {
    this.$root.$on("loginName", userName => {
      this.username = userName;
    });
  }
};
</script>

<style scoped>
.list {
  color: #fff;
  transition: all 0.1s ease-in;
  text-decoration: none;
}
.list:hover {
  -webkit-stroke-width: 5.3px;
  -webkit-stroke-color: #ffffff;
  -webkit-fill-color: #ffffff;
  text-shadow: 5px 0px 20px #fff;
}
</style>