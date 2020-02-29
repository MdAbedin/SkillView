<template>
    <v-card flat>
        <v-list-item three-line>
            <v-list-item-content>
                <div class="overline mb-4" style="font-size: 15px !important;">
                    {{ info.companyName }}
                </div>
                <v-list-item-title class="display-1 font-weight-light mb-1">
                    {{ info.position }}
                </v-list-item-title>
                <v-list-item-subtitle>
                    <v-chip v-for="skill in info.matchedSkills"
                            :key="skill"
                            class="ma-2"
                            color="success"
                            outlined
                            medium
                    >
                        <v-icon medium left>check_circle_outline</v-icon>
                        {{ skill }}
                    </v-chip>


                </v-list-item-subtitle>
                <div class="overline" style="font-size: 10px !important;">   {{ info.location }}</div>

            </v-list-item-content>

            <v-list-item-avatar

                    size="80"
                    color="green"

            >
                <span class="display-1 white--text"> {{info.numMatched}}</span>
            </v-list-item-avatar>
        </v-list-item>
    </v-card>
</template>

<script>
    /**
     * position: 'Full Stack Developer',
     logoURL: '',
     numMatched: Math.floor(Math.random() * 30),
     companyName: 'JP Morgan Chase',
     location: 'New York City, NY',
     matchedSkills: ['C++', 'Python', 'ReactJS'],
     minPay: 12 + Math.floor(Math.random() * 30),
     maxPay: 20 + Math.floor(Math.random() * 30)
     */

    export default {
        name: "listing",
        // get an object from the top
        props: {
            info: {
                type: Object
            }
        }
    }
</script>

<style scoped>

</style>