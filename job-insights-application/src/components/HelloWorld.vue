<template>
  <v-container bg grid-list-md text-center fluid>
    <v-layout row wrap align-center>
      <v-flex class="text-center" md12>
        <v-card >
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

          <!--this is the content for the tabs-->
          <v-tabs-items v-model="tab">
            <v-tab-item :key="1" value="upload">
              <!-- this holds all of the upload logic-->
              <v-card flat>
                <v-card-text>
                  <!--put the image Upload logic here-->
                  <v-layout row>
                    <v-flex>
                      <ImageUpload>
                        <div slot="activator">
                            <v-card flat outlined>
                              <v-card-text>
                                <v-row align="center" justify="center">
                                  <v-col cols="2">
                                    <v-img src="https://res.cloudinary.com/cheezitromansh/image/upload/v1582959082/icons/cloud-computing_1_zt0k9x.png"
                                           lazy-src="https://res.cloudinary.com/cheezitromansh/image/upload/v1582959082/icons/cloud-computing_1_zt0k9x.png"
                                           width="128"
                                    >

                                    </v-img>
                                  </v-col>
                                  <v-col>
                                    <span class="headline font-weight-medium">
                                      Add Files
                                    </span>
                                    <span class="headline font-weight-light">
                                      or drop them in here
                                    </span>
                                  </v-col>
                                </v-row>
                              </v-card-text>
                            </v-card>
                        </div>
                      </ImageUpload>
                    </v-flex>

                    <v-flex>
                      <v-card>
                        <v-card-text>
                          This should be on the right
                        </v-card-text>
                      </v-card>
                    </v-flex>
                  </v-layout>
                </v-card-text>
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
  import ImageUpload from "../components/ImageUpload";

export default {
  name: "HelloWorld",
  components: {
    ImageUpload
  },
  data () {
    return {
      tab: null,

    }
  }
};
</script>
