<template>
  <v-navigation-drawer v-model="drawer" absolute fixed clipped>
    <v-toolbar dark color="primary">
      <v-btn icon dark @click="drawerFalse">
        <v-icon>close</v-icon>
      </v-btn>
      <v-toolbar-title>Bookstore</v-toolbar-title>
    </v-toolbar>



    <v-list v-if="guest">
      <v-list-item>
        <v-btn @click="register()" depressed block rounded color="secondary" class="white--text">
          Register
          <v-icon right dark>person_add</v-icon>
        </v-btn>
      </v-list-item>
      <v-list-item>
        <v-btn @click="login()" block rounded depressed color="accent lighten-1" class="white--text">
          Login
          <v-icon right dark>lock_open</v-icon>
        </v-btn>
      </v-list-item>
    </v-list>
    <v-list v-if="!guest">
      <v-list-item>
        <v-list-item-avatar>
          <img v-if="user.avatar == null" :src="getImage('/notfound.jpg')">
          <img v-else :src="getImage('/users/' + user.avatar)">
        </v-list-item-avatar>
        <v-list-item-content>
          <v-list-item-title>
            {{ user.name }}
          </v-list-item-title>
        </v-list-item-content>
      </v-list-item>
      <v-list-item>
        <v-btn block small rounded depressed color="error lighten-1" class="white--text" @click.stop="
          logout();">
          Logout
          <v-icon small right dark>settings_power</v-icon>
        </v-btn>
      </v-list-item>
    </v-list>


    <v-list class="pt-0" dense>
      <v-divider></v-divider>

      <v-list-item v-for="(item, index) in items" :key="index" :href="item.route" :to="{ name: item.route }">
        <v-list-item-action>
          <v-icon>{{ item.icon }}</v-icon>
        </v-list-item-action>
        <v-list-item-content>
          <v-list-item-title>{{ item.title }}</v-list-item-title>
        </v-list-item-content>
      </v-list-item>
    </v-list>
  </v-navigation-drawer>
</template>
  
<script>
import { mapGetters, mapActions } from 'vuex';

export default {
  name: 'c-side-bar',
  data: () => ({
    items: [
      { title: 'Home', icon: 'dashboard', route: 'home' },
      { title: 'About', icon: 'question_answer', route: 'about' },
    ]
  }),
  computed: {
    ...mapGetters({
      sideBar: 'sideBar',
      user: 'auth/user',
      guest: 'auth/guest',
    }),
    drawer: {
      get() {
        return this.sideBar
      },
      set(value) {
        this.setSideBar(value)
      }
    },
  },
  methods: {

    getImage(image) {
      if (image != null && image.length > 0) {
        // Menggunakan base URL dari variabel VUE_APP_BACKEND_URL
        return "http://bookstore-api.test/image" + image
      }

      return "/img/unavailable.jpg";
    },

    ...mapActions({
      setSideBar: 'setSideBar',
      setStatusDialog: 'dialog/setStatus',
      setComponent: 'dialog/setComponent',
      setAuth: 'auth/set',
      setAlert: 'alert/set',
    }),
    login() {
      this.setStatusDialog(true)
      this.setComponent('login')
      this.setSideBar(false)
    },
    register() {
      this.setStatusDialog(true)
      this.setComponent('register')
      this.setSideBar(false)
    },
    logout() {
      let config = {
        headers: {
          'Authorization': 'Bearer ' + this.user.api_token,
        },
      }
      this.axios.post('/logout', {}, config)
        .then(() => {
          this.setAuth({})
          this.setAlert({
            status: true,
            text: 'Logout succesfuly',
            type: 'success',
          })
          this.setSideBar(false)
        })
        .catch((error) => {
          let response = error.response
          this.setAlert({
            status: true,
            text: response.data.message,
            type: 'error',
          })
        })
    },
    drawerFalse() {
      this.drawer = false;
    },
  },
}
</script>
  