<template>
  <v-app>
    <v-app-bar app color="#fff">
      <div class="d-flex align-center">
        <!-- keep this inplace of the logo-->
        <v-img src="https://res.cloudinary.com/cheezitromansh/image/upload/v1582994098/logo-transparent_bknnbu.png"
               lazy-src="https://res.cloudinary.com/cheezitromansh/image/upload/v1582994098/logo-transparent_bknnbu.png"
               width="64"></v-img>

        <span class="ml-4 headline text-uppercase font-weight-light">
          <span class="font-weight-bold">S</span>kill<span class="font-weight-bold">V</span>iew
        </span>
      </div>

      <v-spacer></v-spacer>

      <v-btn outlined color="purple">
        Add Experience
      </v-btn>

    </v-app-bar>

    <v-content>
      <!-- router link goes here-->
      <HelloWorld />
    </v-content>
  </v-app>
</template>

<script>
import HelloWorld from "./components/HelloWorld";

export default {
  name: "App",

  components: {
    HelloWorld
  },

  data: () => ({
    //
  })
};
</script>
