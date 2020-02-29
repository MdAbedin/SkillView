<template>
  <v-container bg grid-list-md text-center fluid>
    <v-layout row wrap align-center>
      <v-flex class="text-center" md12>
        <v-card>
          <v-tabs
                  v-model="tab"
                  background-color="deep-purple accent-4"
                  centered
                  dark
                  icons-and-text
          >
            <v-tabs-slider></v-tabs-slider>

            <v-tab href="#upload">
              Upload Profile
              <v-icon>mdi-phone</v-icon>
            </v-tab>

            <v-tab href="#viewjobs">
              View Jobs
              <v-icon>mdi-heart</v-icon>
            </v-tab>
          </v-tabs>

          <v-tabs-items v-model="tab">
            <v-tab-item :key="1" value="upload">
              <v-card flat>
                <v-card-text>upload</v-card-text>
              </v-card>
            </v-tab-item>
            <v-tab-item :key="2" value="viewjobs">
              <v-card flat>
                <v-card-text>viewjobs</v-card-text>
              </v-card>
            </v-tab-item>
          </v-tabs-items>
        </v-card>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
export default {
  name: "HelloWorld",
  data () {
    return {
      tab: null,

    }
  }
};
</script>
