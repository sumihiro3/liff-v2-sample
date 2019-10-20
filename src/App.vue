<template>
  <v-app id="app">
    <v-app-bar app>
      <v-toolbar-title class="headline">
        <span>LIFF V2</span>
        <span class="font-weight-light"> Sample</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn large color="error" @click="doLogout" v-show="loggedIn === true && inClient === false">Sign out</v-btn>
    </v-app-bar>

    <v-content>
      <v-layout text-center wrap>
        <!-- LINE Logo -->
        <v-flex xs12>
          <v-img
            :src="require('./assets/LINE_APP.png')"
            class="my-10"
            contain
            height="200"
          ></v-img>
        </v-flex>
        <!-- Component -->
        <!-- <keep-alive> -->
          <LiffV2Login v-if="loggedIn === false" />
          <LiffV2Function v-else />
        <!-- </keep-alive> -->
        <!-- Document link -->
        <v-flex mb-5 xs12>
          <h2 class="headline font-weight-bold mb-3">Status</h2>
          <v-layout justify-center>
            <ul>
              <li>LIFF Initialized: {{ initialized }}</li>
              <li>Logged in: {{ loggedIn }}</li>
              <li>In App browser: {{ inClient }}</li>
            </ul>
          </v-layout>
        </v-flex>
      </v-layout>
    </v-content>
  </v-app>
</template>

<script>
import LiffV2Function from './components/LiffV2Function'
import LiffV2Login from './components/LiffV2Login'

export default {
  name: 'App',
  components: {
    LiffV2Function,
    LiffV2Login,
  },
  data: () => ({
    api_loading: false,
    initialized: false,
    loggedIn: false,
    inClient: false
  }),
  created: function() {
    console.log(`process.env.VUE_APP_LIFF_ID: ${process.env.VUE_APP_LIFF_ID}`)
  },
  mounted: function() {
    this.initializeLiff()
    this.initVConsole()
  },
  methods: {
    initializeLiff: function () {
      this.api_loading = true
      liff.init(
        {
          liffId: process.env.VUE_APP_LIFF_ID
        },
        data => {
          this.initialized = true
          this.loggedIn = liff.isLoggedIn()
          this.inClient = liff.isInClient()
          if (this.loggedIn === true) {
            // ログイン済み
            console.log('Logged in!!')
          } else {
            console.log('Not logged in!!')
          }
          this.api_loading = false
        },
        err => {
          console.log('LIFF initialization failed', err)
          this.api_loading = false
        }
      )
    },
    initVConsole: async function() {
      window.vConsole = new window.VConsole({
          defaultPlugins: ['system', 'network', 'element', 'storage'],
          maxLogNumber: 1000,
          onReady: function () {
              console.log('vConsole is ready.')
          },
          onClearLog: function () {
              console.log('on clearLog')
          }
      })
    },
    doLogout: function() {
      if (liff.isLoggedIn()) {
        liff.logout()
        this.loggedIn = false
        window.location.reload()
      }
    }
  }
}
</script>
