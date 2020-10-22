<template>
    <div>  
        <v-layout row wrap>       
            <!-- <v-flex xs12 pl-2 row class="hidden-md-and-down">
                <v-layout row wrap>
                <p style="color: #27304c; font-size 6px;" class="pl-2 pr-2">
                    Select an analisys to run for the coordinates showed on top of the map. Then click the "run" button to display the result. 
                </p>
                </v-layout>
            </v-flex>                     -->

            <v-flex sm9 xs9 md9 class="pl-3 pr-3">
                <v-combobox style="margin-top: 0px; padding-top: 0px"
                v-model="selectedMbService"
                :items="mbServices"
                item-text="name"
                item-value="value"
                label="Select service"
                color="green"
                ></v-combobox>
            </v-flex>
                    
            <v-btn icon @click="infoDialog = true" class="ml-3" title="More info" v-if="selectedMbService">
                <v-icon color="#27304c">help_outline</v-icon>
            </v-btn>

            <v-flex xs12 pl-2 row v-if="selectedMbService.value == 'seasonAverages'">
                <SeasonAverages/>
            </v-flex>

            <v-flex xs12 pl-2 row v-if="selectedMbService.value == 'yearlyAverages'">
                <YearlyAverages/>
            </v-flex>

            <v-flex xs12 pl-2 row v-if="selectedMbService.value == 'eventDailyDistributions'">
                <EventDailyDistributions/>
            </v-flex>

            <v-flex xs12 pl-2 row v-if="selectedMbService.value == 'eventHourlyDistributions'">
                <EventHourlyDistributions/>
            </v-flex>

        </v-layout>

        <!------------ Info dialog ------------>
        <v-dialog v-model="infoDialog" max-width="600">
            <v-card> 
                <v-card-title class="headline">
                    <img style="width: 130px;" src="../assets/logo_titulo.png" alt="">
                </v-card-title>

                <v-divider></v-divider>
                <v-card-text>

                    <v-flex xs12 v-if="selectedMbService.value == 'seasonAverages'">
                        <v-card flat class="pa-2">
                                                
                            <div style="margin-bottom: 15px; margin-top: 15px">
                                <img style="width: 160px; position: absolute; opacity: 0.2; bottom: 5px; right: 5px;" src="../assets/logo2.png" alt="">
                                <span class="title" style="color: #1e2f4d;">Crop climate risk comparison</span>
                            </div> 
                            <p>
                                The Crop climate risk comparison diagrams show the comparison of the main weather variables (temperature, precipitation,
                                evapotranspiration, radiation) averaged on customisable time periods. Based on more than 30 years of historical simulation data
                                for any location on earth.
                            </p>
                            <p>
                                <span class="title" style="color: #37aa48; font-size 12px;">
                                    Season averages
                                </span>
                            </p>
                            <p>
                                The Season averages diagram shows the comparison, along a customisable growing season, of average curves of the selected weather
                                variable relative to two different time periods (typically the last 10 years vs 10 years of the past). The distributions compare
                                the Kernel Density Estimation for the two selected time periods.
                            </p>
                        </v-card>
                    </v-flex>

                    <v-flex xs12 v-if="selectedMbService.value == 'yearlyAverages'">
                        <v-card flat class="pa-2">
                                                
                            <div style="margin-bottom: 15px; margin-top: 15px">
                                <img style="width: 160px; position: absolute; opacity: 0.2; bottom: 5px; right: 5px;" src="../assets/logo2.png" alt="">
                                <span class="title" style="color: #1e2f4d;">Crop climate risk comparison</span>
                            </div> 
                            <p>
                                The Crop climate risk comparison diagrams show the comparison of the main weather variables (temperature, precipitation,
                                evapotranspiration, radiation) averaged on customisable time periods. Based on more than 30 years of historical simulation data
                                for any location on earth.
                            </p>
                            <p>
                                <span class="title" style="color: #37aa48; font-size 12px;">
                                    Yearly averages
                                </span>
                            </p>
                            <p>
                                The Yearly averages diagram shows the comparison of the yearly values of the selected weather variable relative to two
                                costumisable time periods (typically the last 10 years vs 10 years of the past). The distributions compare the Kernel Density
                                Estimation for the two selected time periods.
                            </p>
                        </v-card>
                    </v-flex>

                    <v-flex xs12 v-if="selectedMbService.value == 'eventDailyDistributions'">
                        <v-card flat class="pa-2">
                                                
                            <div style="margin-bottom: 15px; margin-top: 15px">
                                <img style="width: 160px; position: absolute; opacity: 0.2; bottom: 5px; right: 5px;" src="../assets/logo2.png" alt="">
                                <span class="title" style="color: #1e2f4d;">Crop climate risk comparison</span>
                            </div> 
                            <p>
                                The Crop climate risk comparison diagrams show the comparison of the main weather variables (temperature, precipitation,
                                evapotranspiration, radiation) averaged on customisable time periods. Based on more than 30 years of historical simulation data
                                for any location on earth.
                            </p>
                            <p>
                                <span class="title" style="color: #37aa48; font-size 12px;">
                                    Daily events
                                </span>
                            </p>
                            <p>
                               The API returns event series meaningful at daily level (e.g. daily maximum temperature above a certain threshold), relative to
                                two costumizable time periods (typically the last 10 years vs 10 years of the past). The diagram compares the Kernel Density
                                Estimation and statistical distributions for the two selected time periods.
                            </p>
                        </v-card>
                    </v-flex>

                    <v-flex xs12 v-if="selectedMbService.value == 'eventHourlyDistributions'">
                        <v-card flat class="pa-2">
                                                
                            <div style="margin-bottom: 15px; margin-top: 15px">
                                <img style="width: 160px; position: absolute; opacity: 0.2; bottom: 5px; right: 5px;" src="../assets/logo2.png" alt="">
                                <span class="title" style="color: #1e2f4d;">Crop climate risk comparison</span>
                            </div> 
                            <p>
                                The Crop climate risk comparison diagrams show the comparison of the main weather variables (temperature, precipitation,
                                evapotranspiration, radiation) averaged on customisable time periods. Based on more than 30 years of historical simulation data
                                for any location on earth.
                            </p>
                            <p>
                                <span class="title" style="color: #37aa48; font-size 12px;">
                                    Hourly events
                                </span>
                            </p>
                            <p>
                                The API returns event series meaningful at hourly level (e.g. 1 hour of precipitation above a certain threshold) in 3 possible
                                aggregations (number of events/season, number of hours/season, number of days/season), relative to two costumizable time periods
                                (typically the last 10 years vs 10 years of the past). The diagram compares the Kernel Density Estimation and statistical
                                distributions for the two selected time periods.
                            </p>
                        </v-card>
                    </v-flex>

                </v-card-text>
            </v-card>
        </v-dialog>
        <!------------ /Info dialog ------------>

        <!------------ Output dialog ------------>
        <v-dialog v-model="outputDialog" max-width="800">
            <v-card v-if = !isLoading> 
                <v-card-title class="headline">
                    <img style="width: 130px;" src="../assets/logo_titulo.png" alt="">
                    <v-spacer></v-spacer>
                    <v-btn icon title="Download graphic" @click="downloadGraph()">
                    <v-icon color="#27304c">archive</v-icon>
                    </v-btn>
                </v-card-title>

                <v-divider></v-divider>
                <v-card-text>
                    <v-flex xs12  class="pa-3" >
                    <v-img :src="grahpURL" alt=""></v-img>
                    </v-flex>
                </v-card-text>
            </v-card>
            <v-card v-if = isLoading>
                <v-card-text style="text-align: center;">
                    <img style="width: 120px;" src="../assets/logo_titulo.png" alt=""><br>
                    <v-progress-circular :size="60" :width="7" color="green" indeterminate style="margin-top: 15px;"></v-progress-circular>
                    <v-flex xs12 style="color: #37aa48; font-size 12px; margin-bottom: 10px; margin-top: 10px;">
                    Processing...
                    </v-flex>
                </v-card-text>
            </v-card>
        </v-dialog>
        <!------------ /Output dialog ------------>

    </div>

</template>

<script>

import SeasonAverages from '@/components/seasonAverages.vue'
import YearlyAverages from '@/components/yearlyAverages.vue'
import EventDailyDistributions from '@/components/eventDailyDistributions.vue'
import EventHourlyDistributions from '@/components/eventHourlyDistributions.vue'

export default {
    name: "RiskComparison",    
    components: {
        SeasonAverages,
        YearlyAverages,
        EventDailyDistributions,
        EventHourlyDistributions
    },
    data: () => ({
        isLoading: false,
        outputDialog: false,
        grahpURL: "",
        infoDialog: false,
        selectedMbService:"",
        mbServices: [
            {
                'name': 'Season averages',
                'value': 'seasonAverages'
            },
            {
                'name': 'Yearly averages ', 
                'value': 'yearlyAverages'
            },
            {
                'name': 'Daily events',
                'value': 'eventDailyDistributions'
            },
            {
                'name': 'Hourly events',
                'value': 'eventHourlyDistributions'
            }
        ],                 
    }),
    methods: {   
        getOutput(url, format){

            this.isLoading = true;            
            this.outputDialog = true;
            this.grahpURL = url;
            var self = this;

            if(format === 'png'){                
                setTimeout(function(){                                                                    
                    self.$eventBus.$emit('show-alert', "success", "Diagram retrieved successfully");   
                    self.isLoading = false;                  
                }, 4000);                           
            }                                    
                            

            console.log(format)

            // this.$http.get(url).then(response => {                                                    
            // }, response => {
            //     this.isLoading = false;                    
            //     this.$eventBus.$emit('show-alert', "error", response.statusText); 
            // });

        },
        /**
        * Open the image in other browser tab to force to be download
        *
        * @public
        */
        downloadGraph(){
            window.open(this.grahpURL)  
        },
    },
    created(){

        this.$eventBus.$on('get-output', (url, format)  => {
            this.getOutput(url, format);
        });

    },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>


