<style>
.fade-enter-active, .fade-leave-active {
  transition: all 2.9s ease;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
.skills {
  transition: transform 5s;
}
</style>

<template>
  <v-container bg grid-list-md text-center fluid>
    <v-dialog
            v-model="dialog"
    >
      <v-card v-if="dialog_info">
        <v-toolbar dark>
          <v-app-bar-nav-icon></v-app-bar-nav-icon>
          <v-toolbar-title class="overline" style="font-size: 15px !important;">
            {{ dialog_info.companyName }}
          </v-toolbar-title>
        </v-toolbar>

        <v-card-title class="display-3 font-weight-light ">
          {{ dialog_info.position }}
        </v-card-title>

        <v-card-text>
          {{ dialog_info.position }}
        </v-card-text>
         <div id="chart">
          <apexchart type="radar" height="350" :options="chartOptions" :series="series"></apexchart>
          </div>

        <v-card-actions>
          <v-spacer></v-spacer>

          <v-btn
                  color="green darken-1"
                  text
                  @click="dialog = false"
          >
            Disagree
          </v-btn>

          <v-btn
                  color="green darken-1"
                  text
                  @click="dialog = false"
          >
            Agree
          </v-btn>
        </v-card-actions>
      </v-card>
      <v-card v-else>
        <v-card-text>
          No data here
        </v-card-text>
      </v-card>
    </v-dialog>
    <v-layout row wrap align-center>
      <v-flex md12>
        <v-card class="text-center">
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
                    <v-flex v-if="uploadedImage">
                      <ImageUpload v-model="uploadedImage">
                        <div slot="activator">
                          <v-card class="elevation-10">
                            <v-img :src="uploadedImage.imageURL" aspect-ratio="1"></v-img>
                          </v-card>
                        </div>
                      </ImageUpload>
                    </v-flex>

                    <v-flex v-else>
                      <ImageUpload v-model="uploadedImage">
                        <div slot="activator">
                          <v-card flat outlined>
                            <v-card-text>
                              <v-row align="center" justify="center">
                                <v-col cols="2">
                                  <v-img src="https://res.cloudinary.com/cheezitromansh/image/upload/v1582959082/icons/cloud-computing_1_zt0k9x.png"
                                         lazy-src="https://res.cloudinary.com/cheezitromansh/image/upload/v1582959082/icons/cloud-computing_1_zt0k9x.png"
                                         width="100px">

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
                        <transition-group appear name="fade">
                        <div v-for="skill in message" :key="skill" class="my-5;">
                        <v-btn outlined block  color=deep-purple accent-4> 
                        {{skill}}
                         </v-btn>

                        </div>
                        </transition-group>
                  
                      </v-card>
                    </v-flex>
                    
                  </v-layout>
                </v-card-text>
              </v-card>
            </v-tab-item>
            <v-tab-item :key="2" value="viewjobs">
              <v-card flat>
                <v-card-text>
                  <v-layout column>
                    <v-flex class="text-left">
                      <!-- list work way better for taking up the tire space-->
                      <v-list >
                        <v-list-item-group >
                          <v-list-item v-for="j in jobs" :key="j.companyName" @click="showDialog(j)">
                            <!-- create a logo -> just be a picture-->
                            <v-list-item-avatar size="150">
                              <v-img :src="j.logoURL"></v-img>
                            </v-list-item-avatar>

                            <v-list-item-content>
                              <Listing :info="j"></Listing>
                            </v-list-item-content>
                          </v-list-item>
                        </v-list-item-group>
                      </v-list>

                    </v-flex>
                  </v-layout>
                </v-card-text>
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
  import Listing from "../components/listing";
  import VueApexCharts from 'vue-apexcharts';

export default {
  name: "HelloWorld",
  components: {
    apexchart:VueApexCharts,
    ImageUpload,
    Listing,
  },
  data () {
    return {
      dialog_info: null,
      dialog: false,
      tab: null,
      message:[],
      uploadedImage: null,
      jobs: [
        {
          position: 'Full Stack Developer',
          logoURL: 'https://res.cloudinary.com/dsnttadso/image/upload/v1582974358/JP-morgan_rx91up.png',
          numMatched: Math.floor(Math.random() * 30),
          companyName: 'JP Morgan Chase',
          location: 'New York City, NY',
          matchedSkills: ['C++', 'Python', 'ReactJS'],
          minPay: 12 + Math.floor(Math.random() * 30),
          maxPay: 20 + Math.floor(Math.random() * 30)

        },
        {
          position: 'Junior Frontend Developer',
          logoURL: 'https://res.cloudinary.com/dsnttadso/image/upload/v1582974358/uber_aw6a9v.jpg',
          numMatched: Math.floor(Math.random() * 30),
          companyName: 'Uber',
          location: 'Santa Monica, CA',
          matchedSkills: ['Vim', 'ReactJS', 'Javascript'],
          minPay: 12 + Math.floor(Math.random() * 30),
          maxPay: 20 + Math.floor(Math.random() * 30)

        },
        {
          position: 'Network Engineer',
          logoURL: 'https://res.cloudinary.com/dsnttadso/image/upload/v1582975324/untitled_p4qikm.png',
          numMatched: Math.floor(Math.random() * 30),
          companyName: 'Apple',
          location: 'Mountain View, CA',
          matchedSkills: ['C', 'Rust', 'NetworksC'],
          minPay: 12 + Math.floor(Math.random() * 30),
          maxPay: 20 + Math.floor(Math.random() * 30)

        },
        {
          position: 'Jr. Data Science Member',
          logoURL: 'https://res.cloudinary.com/dsnttadso/image/upload/v1582975324/abbott-laboratories_416x416_sm4vpe.jpg',
          numMatched: Math.floor(Math.random() * 30),
          companyName: 'Abbott Labs',
          location: 'Irving, TX',
          matchedSkills: ['Python', 'Julia', 'R'],
          minPay: 12 + Math.floor(Math.random() * 30),
          maxPay: 20 + Math.floor(Math.random() * 30)

        },
        {
          position: 'Video Game Designer',
          logoURL: 'https://res.cloudinary.com/dsnttadso/image/upload/v1582976579/blizzard-entertainment-logo-11530958317pvsb2iytsk_bsvsyl.png',
          numMatched: Math.floor(Math.random() * 30),
          companyName: 'Blizzard Entertainment',
          location: 'Brisbane, Australia',
          matchedSkills: ['C++', 'C#', 'Unity'],
          minPay: 12 + Math.floor(Math.random() * 30),
          maxPay: 20 + Math.floor(Math.random() * 30)

        },
        {
          position: 'Backend Developer',
          logoURL: 'https://res.cloudinary.com/dsnttadso/image/upload/v1582975623/75cca60559bdb1d019238e21dffb1eef_gk9o7r.png',
          numMatched: Math.floor(Math.random() * 30),
          companyName: 'Google',
          location: 'Santa Monica, CA',
          matchedSkills: ['C++', 'Python', 'Rustlang'],
          minPay: 12 + Math.floor(Math.random() * 30),
          maxPay: 20 + Math.floor(Math.random() * 30)
        },


      ],
      skills: [
      {type: 'Javascript'},
      {type: 'HTML'},
      {type: 'Vue.JS'},
      {type: 'Unity'},
      {type: 'MongoDB'},
      {type: 'C++'}
    ],
    series: [{
            name: 'Series 1',
            data: [60]
          }],
          chartOptions: {
            chart: {
              height: 350,
              type: 'radar',
            },
            dataLabels: {
              enabled: true
            },
            plotOptions: {
              radar: {
                size: 140,
                polygons: {
                  strokeColor: '#e9e9e9',
                  fill: {
                    colors: ['#f8f8f8', '#fff']
                  }
                }
              }
            },
            title: {
              text: 'Radar with Polygon Fill'
            },
            colors: ['#FF4560'],
            markers: {
              size: 4,
              colors: ['#fff'],
              strokeColor: '#FF4560',
              strokeWidth: 2,
            },
            tooltip: {
              y: {
                formatter: function(val) {
                  return val
                }
              }
            },
            xaxis: {
              categories: ['Javascript', 'HTML', 'React Native', 'Vue.JS', 'MongoDB', 'C++', 'Unity']
            },
            yaxis: {
              tickAmount: 7,
              labels: {
                formatter: function(val, i) {
                  if (i % 2 === 0) {
                    return val
                  } else {
                    return ''
                  }
                }
              }
            }
          },
    }
    
  },
  methods:{
        showSkills: function () {
          let v = this;
          let i = 0;
          var loop=setInterval(function () {
            v.message.push(v.skills[i].type);
            i=i+1;
            if (v.skills.length === i)
            {
              clearInterval(loop);
            }
          }, 2000);
        },
    showDialog: function (info_obj) {
          this.dialog = true;
          this.series[0].data=[];

          // set the info that the dialog will have
          this.dialog_info = info_obj;
          for(let i =0; i<8;i++){
            this.series[0].data.push(Math.floor(Math.random() * (10 - 1 + 1)) + 1);
          }
              },
                
                      
    }


 
};
</script>
