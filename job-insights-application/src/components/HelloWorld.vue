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
        <v-toolbar dark color="deep-purple">
          <v-app-bar-nav-icon></v-app-bar-nav-icon>
          <v-toolbar-title class="overline" style="font-size: 15px !important;">
            {{ dialog_info.companyName }}
          </v-toolbar-title>
        </v-toolbar>

        <v-card-title class="display-3 font-weight-light ">
          {{ dialog_info.position }}
        </v-card-title>

        <v-card-subtitle>
          {{ dialog_info.location }}
        </v-card-subtitle>

        <v-card-text>
          <v-row justify="center">
            <v-col>
              <div id="chart">
                <apexchart type="radar" height="550" :options="chartOptions" :series="series"></apexchart>
              </div>
            </v-col>
            <v-col>
              <!-- this contains the information -->
              <v-card flat>
                <v-list>
                  <v-list-item-group>
                    <v-list-item>
                      <v-list-item-content three-line>
                        <v-list-item-title>
                          <span class="headline font-weight-light">
                            Skills Matched
                          </span>
                        </v-list-item-title>
                        <v-list-item-subtitle>
                          <v-chip color="success"
                                  outlined
                                  medium
                                  class="ma-2"
                                  v-for="c in dialog_info.matchedSkills" :key="c">
                            <v-icon medium left>check_circle_outline</v-icon>
                            {{ c }}
                          </v-chip>
                        </v-list-item-subtitle>
                      </v-list-item-content>
                    </v-list-item>

                    <v-list-item>
                      <v-list-item-content three-line>
                        <v-list-item-title>
                          <span class="headline font-weight-light">
                            Unmet Skills
                          </span>
                        </v-list-item-title>
                        <v-list-item-subtitle>
                          <v-chip color="error"
                                  outlined
                                  medium
                                  class="ma-2"
                                  v-for="c in getUnmetSkills(dialog_info.matchedSkills)" :key="c">
                            <v-icon medium left>check_circle_outline</v-icon>
                            {{ c }}
                          </v-chip>
                        </v-list-item-subtitle>
                      </v-list-item-content>
                    </v-list-item>

                    <v-list-item>
                      <v-list-item-content>
                        <!-- we could put a whole card here-->
                        <v-card
                                flat
                                class="mx-auto"
                        >
                          <v-card-title>
                            <div class="headline grey--text text-uppercase">
                              Pay Over Time
                            </div>
                          </v-card-title>

                          <v-card-text>
                            <v-row>

                              <div>
                                <strong v-if="avg">$</strong>
                                <span
                                    class="display-2 font-weight-black"
                                    v-text="avg || '—'"
                                ></span>

                              </div>
                            </v-row>
                          </v-card-text>

                          <v-sheet color="transparent">
                            <v-sparkline
                                    :key="String(avg)"
                                    :smooth="16"
                                    :gradient="['#f72047', '#ffd200', '#1feaea']"
                                    :line-width="3"
                                    :value="heartbeats"
                                    :labels="labels"
                                    label-size="3"
                                    auto-draw
                                    stroke-linecap="round"
                            ></v-sparkline>
                          </v-sheet>
                        </v-card>
                      </v-list-item-content>
                    </v-list-item>
                  </v-list-item-group>
                </v-list>
              </v-card>
            </v-col>
          </v-row>
        </v-card-text>


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
                      <v-btn @click="showSkills()" block  color=deep-purple accent-4> 
                      Click to Analyze Resume
</v-btn>
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
                          <v-list-item v-for="j in jobs" :key="j.companyName" @click="showDialog(j), takePulse()">
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

  const exhale = ms =>
          new Promise(resolve => setTimeout(resolve, ms));

export default {
  name: "HelloWorld",
  components: {
    apexchart:VueApexCharts,
    ImageUpload,
    Listing,
  },
  data () {
    return {
      labels: ['02/18','03/18', '04/18', '05/18', '06/18', '07/18', '08/18','09/18', '10/18','11/18','12/18', '01/19', '02/19', '03/19', '04/19', '05/19', '06/19', '07/19', '08/19', '09/19'],
      checking: false,
      heartbeats: [],
      dialog_info: null,
      dialog: false,
      tab: null,
      message:[],
      uploadedImage: null,
      skillsList: ['C++', 'C', 'HTML', 'Javascript', 'VueJS', 'Rustlang', 'Unity', 'MongoDB', 'ReactNative'],
      jobs: [
        {
          position: 'Full Stack Developer',
          logoURL: 'https://res.cloudinary.com/dsnttadso/image/upload/v1582974358/JP-morgan_rx91up.png',
          numMatched: 3 + Math.floor(Math.random() * 10),
          companyName: 'JP Morgan Chase',
          location: 'New York City, NY',
          matchedSkills: ['C++', 'Python', 'ReactJS'],
          minPay: 12 + Math.floor(Math.random() * 30),
          maxPay: 20 + Math.floor(Math.random() * 30)

        },
        {
          position: 'Junior Frontend Developer',
          logoURL: 'https://res.cloudinary.com/dsnttadso/image/upload/v1582974358/uber_aw6a9v.jpg',
          numMatched: 3 + Math.floor(Math.random() * 10),
          companyName: 'Uber',
          location: 'Santa Monica, CA',
          matchedSkills: ['Vim', 'ReactJS', 'Javascript'],
          minPay: 12 + Math.floor(Math.random() * 30),
          maxPay: 20 + Math.floor(Math.random() * 30)

        },
        {
          position: 'Network Engineer',
          logoURL: 'https://res.cloudinary.com/dsnttadso/image/upload/v1582975324/untitled_p4qikm.png',
          numMatched: 3 + Math.floor(Math.random() * 10),
          companyName: 'Apple',
          location: 'Mountain View, CA',
          matchedSkills: ['C', 'Rust', 'NetworksC'],
          minPay: 12 + Math.floor(Math.random() * 30),
          maxPay: 20 + Math.floor(Math.random() * 30)

        },
        {
          position: 'Jr. Data Science Member',
          logoURL: 'https://res.cloudinary.com/dsnttadso/image/upload/v1582975324/abbott-laboratories_416x416_sm4vpe.jpg',
          numMatched: 3 + Math.floor(Math.random() * 10),
          companyName: 'Abbott Labs',
          location: 'Irving, TX',
          matchedSkills: ['Python', 'Julia', 'R'],
          minPay: 12 + Math.floor(Math.random() * 30),
          maxPay: 20 + Math.floor(Math.random() * 30)

        },
        {
          position: 'Video Game Designer',
          logoURL: 'https://res.cloudinary.com/dsnttadso/image/upload/v1582976579/blizzard-entertainment-logo-11530958317pvsb2iytsk_bsvsyl.png',
          numMatched: 3 + Math.floor(Math.random() * 10),
          companyName: 'Blizzard Entertainment',
          location: 'Brisbane, Australia',
          matchedSkills: ['C++', 'C#', 'Unity'],
          minPay: 12 + Math.floor(Math.random() * 30),
          maxPay: 20 + Math.floor(Math.random() * 30)

        },
        {
          position: 'Backend Developer',
          logoURL: 'https://res.cloudinary.com/dsnttadso/image/upload/v1582975623/75cca60559bdb1d019238e21dffb1eef_gk9o7r.png',
          numMatched: 3 + Math.floor(Math.random() * 10),
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
                size: 240,
                polygons: {
                  strokeColor: '#e9e9e9',
                  fill: {
                    colors: ['#f8f8f8', '#fff']
                  }
                }
              }
            },
            title: {
              text: ''
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
              categories: ['Javascript', 'HTML', 'React Native', 'Vue.JS', 'MongoDB', 'C++', 'Unity', 'Rustlang']
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
          }, 500);
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

    getUnmetSkills: function (notIncluded) {
      let newArray = this.skillsList;

      for(let i = 0; i < newArray.length; i++){
        if(notIncluded.includes(newArray[i])){
          newArray.splice(i, 1);
          i--;
        }
      }

      console.log(newArray)

      return newArray

    },
    heartbeat: function () {
      return Math.ceil(Math.random() * (1000 - 200) + 80)
    },
    takePulse: async function (inhale = true) {
      this.checking = true

      inhale && await exhale(500)

      this.heartbeats = Array.from({ length: 20 }, this.heartbeat)

      this.checking = false
    }
                
                      
    },
  computed: {
    avg () {
      const sum = this.heartbeats.reduce((acc, cur) => acc + cur, 0)
      const length = this.heartbeats.length

      if (!sum && !length) return 0


      return (Math.ceil(sum / length) * 100).toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
    },
  }


 
};
</script>
