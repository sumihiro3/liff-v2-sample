<template>
  <v-container>
    <v-layout text-center wrap>
      <v-flex mb-8 xs12>
        <h1 class="display-2 font-weight-bold mb-3">LIFF v2 Sample</h1>
        <p
          class="subheading font-weight-regular"
        >Please press below buttons to try the LIFF v2 APIs.</p>
      </v-flex>
      <v-flex mb-8 xs12>
        <h3>Welcome !!</h3>
        <h3>
          <v-avatar color="teal" size="48" v-show="loggedIn === true">
            <img :src="pictureUrl" />
          </v-avatar>
          {{name}}
        </h3>
      </v-flex>
      <v-flex mb-4 xs6>
        <!-- getAccessToekn -->
        <div class="ma-4">
          <v-btn
            rounded
            class="custom-transform-class text-none"
            color="primary"
            @click="getAccessToken"
            :disabled="loggedIn === false"
          >getAccessToekn</v-btn>
        </div>
        <!-- getLanguage -->
        <div class="ma-4">
          <v-btn
            rounded
            class="custom-transform-class text-none"
            color="primary"
            @click="getLanguage"
            :disabled="loggedIn === false"
          >getLanguage</v-btn>
        </div>
        <!-- getOS -->
        <div class="ma-4">
          <v-btn 
            rounded 
            class="custom-transform-class text-none"
            color="primary" 
            @click="getOS" 
            :disabled="loggedIn === false"
          >getOS</v-btn>
        </div>
        <!-- getVersion -->
        <div class="ma-4">
          <v-btn
            rounded
            class="custom-transform-class text-none"
            color="primary"
            @click="getVersion"
            :disabled="loggedIn === false"
          >getVersion</v-btn>
        </div>
      </v-flex>
      <v-flex mb-4 xs6>
        <!-- openWindow -->
        <div class="ma-4">
          <v-btn
            rounded
            class="custom-transform-class text-none"
            color="success"
            @click="openWindow"
            :disabled="loggedIn === false"
          >openWindow</v-btn>
        </div>
        <!-- sendMessages -->
        <div class="ma-4">
          <v-btn
            rounded 
            class="custom-transform-class text-none"
            color="success"
            @click="sendMessages"
            :disabled="loggedIn === false || inClient === false"
          >sendMessage</v-btn>
        </div>
        <!-- scanCode -->
        <div class="ma-4">
          <v-btn
            rounded 
            class="custom-transform-class text-none"
            color="success"
            @click="scanCode"
            :disabled="loggedIn === false || inClient === false"
          >scanCode</v-btn>
        </div>
      </v-flex>
      <!-- Snackbar -->
      <div>
        <v-snackbar
          v-model="snackbarFlag"
          color="primary"
          multi-line
          :timeout="snackbarTimeout"
          top
        >
          {{ snackbarText }}
        </v-snackbar>
      </div>
    </v-layout>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    snackbarFlag: false,
    snackbarTimeout: 5000,
    snackbarText: '',
    loggedIn: false,
    inClient: false,
    name: "",
    pictureUrl: "",
    accessToken: "",
    os: "",
    language: "",
    liff_version: ""
  }),
  created: function() {
    this.loggedIn = liff.isLoggedIn()
    this.inClient = liff.isInClient()
    liff
      .getProfile()
      .then(profile => {
        this.name = profile.displayName
        this.pictureUrl = profile.pictureUrl
      })
      .catch(err => {
        console.log(`Error at getProfile: ${err}`)
        this.name = ""
      })
  },
  mounted: function() {},
  methods: {
    getAccessToken: function() {
      this.accessToken = liff.getAccessToken()
      const text = `Your Access Token is [${this.accessToken}]`
      console.log(text)
      this.showSnackbar(text)
    },
    getOS: function() {
      this.os = liff.getOS()
      const text = `Your OS is [${this.os}]`
      console.log(text)
      this.showSnackbar(text)
    },
    getLanguage: function() {
      this.language = liff.getLanguage()
      const text = `Your Language is [${this.language}]`
      console.log(text)
      this.showSnackbar(text)
    },
    getVersion: function() {
      this.liff_version = liff.getVersion()
      const text = `LIFF Version is [${this.liff_version}]`
      console.log(text)
      this.showSnackbar(text)
    },
    sendMessages: function() {
      const text = `I am ${this.name} !!`
      const msg = `Message is [${text}]`
      console.log(msg)
      this.showSnackbar(msg)
      liff
        .sendMessages([
          {
            type: "text",
            text: text
          }
        ])
        .then(() => {
          console.log("message sent")
        })
        .catch(err => {
          console.log("error", err)
        })
    },
    openWindow: function() {
      const url = "https://line.me"
      const text = `Open [${url}]`
      console.log(text)
      this.showSnackbar(text)
      liff.openWindow({
        url: "https://line.me",
        external: this.inClient
      })
    },
    scanCode: function() {
      liff
        .scanCode().then(result => {
          console.log(`${JSON.stringify(result)}`)
          const text = `The code represents [${result.value}]`
          console.log(text)
          this.showSnackbar(text)
        })
        .catch(err => {
          console.log("error", err)
        })
    },
    showSnackbar(text) {
        this.snackbarFlag = true
        this.snackbarText = text
        setTimeout(()=> {
            this.snackbarFlag = false
            this.snackbarText = ''
          },
          this.snackbarTimeout
        )
      },
  }
}
</script>
