<template>
  <v-app>
    <v-app-bar app>
      <div class="d-flex align-center">
        <!-- keep this inplace of the logo-->
        <v-avatar color="indigo">
          <v-icon dark>home</v-icon>
        </v-avatar>

        <span class="ml-4 headline font-weight-light">
          SkillView
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
