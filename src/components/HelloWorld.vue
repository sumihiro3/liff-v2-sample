<template>
  <v-container>
    <v-layout
      text-center
      wrap
    >
      <v-flex xs12>
        <v-img
          :src="require('../assets/LINE_APP.png')"
          class="my-10"
          contain
          height="200"
        ></v-img>
      </v-flex>

      <v-flex mb-4>
        <h1 class="display-2 font-weight-bold mb-3">
          Welcome to LIFF v2 Sample page
        </h1>
        <p class="subheading font-weight-regular">
          LIFF (LINE Front-end Framework) is the web application framework for LINE app.
          <br>For help please visit the official documentation page
          <a href="https://developers.line.biz/ja/docs/liff/" target="_blank">LINE Front-end Framework documentation</a>
        </p>
      </v-flex>

      <v-flex
        mb-5
        xs12
      >
        <h2 class="headline font-weight-bold mb-3">What's next?</h2>

        <v-layout justify-center>
          <a
            v-for="(next, i) in whatsNext"
            :key="i"
            :href="next.href"
            class="subheading mx-3"
            target="_blank"
          >
            {{ next.text }}
          </a>
        </v-layout>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    whatsNext: [
      {
        text: 'LINE Engineering blog',
        href: 'https://engineering.linecorp.com/ja/blog/liff-v2/',
      },
      {
        text: 'About LIFF',
        href: 'https://engineering.linecorp.com/ja/blog/liff-our-latest-product-for-third-party-developers/',
      },
      {
        text: 'LIFF v2 API Reference',
        href: 'https://developers.line.biz/ja/reference/liff/',
      },
    ],
  }),
};
</script>
