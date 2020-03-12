<template>
  <v-container>
    <v-layout text-center wrap>
      <v-flex mb-4>
        <p class="subheading font-weight-regular">
          Please sign in by LINE Login.
        </p>
        <h3>
          <v-btn large color="success" @click="doLogin">Sign in</v-btn>
        </h3>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
export default {
  data: () => ({
  }),
  created: function() {
  },
  mounted: function() {
  },
  methods: {
    doLogin: function () {
      liff.login({
        redirectUri: "https://liff-v2-sample.netlify.com/?hogehoge=fugafuga"
      })
    }
  }
};
</script>
