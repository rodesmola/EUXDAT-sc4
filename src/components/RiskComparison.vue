<template>
    <div class="pl-2">  
        <v-layout row wrap class="pl-2 pr-2">       

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
                                {{cropClimateText}}
                            </p>
                            <p>
                                <span class="title" style="color: #37aa48; font-size 12px;">
                                    {{selectedMbService.name}}
                                </span>
                            </p>
                            <p>
                                {{seasonAveragesText}}
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
                                {{cropClimateText}}
                            </p>
                            <p>
                                <span class="title" style="color: #37aa48; font-size 12px;">
                                    {{selectedMbService.name}}
                                </span>
                            </p>
                            <p>
                                {{yearlyAveragesText}}
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
                                {{cropClimateText}}
                            </p>
                            <p>
                                <span class="title" style="color: #37aa48; font-size 12px;">
                                    {{selectedMbService.name}}
                                </span>
                            </p>
                            <p>
                               {{eventDailyDistributionsText}}
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
                                {{cropClimateText}}
                            </p>
                            <p>
                                <span class="title" style="color: #37aa48; font-size 12px;">
                                    {{selectedMbService.name}}
                                </span>
                            </p>
                            <p>
                                {{eventHourlyDistributionsText}}
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

                    <v-btn icon title="Download diagram" @click="downloadOutput(format)" v-if="format === 'png'">
                        <v-icon color="#27304c">archive</v-icon>
                    </v-btn>
                    <v-flex xs2 v-if="format === 'json'">
                        <v-btn dark small block @click="downloadOutput(format)" style="background-color: #47a34b; color: white;" title="Download JSON">
                            <v-icon class="pa-1 ml-1">save</v-icon> <span v-if="!$vuetify.breakpoint.xs">Download</span>
                        </v-btn>        
                    </v-flex>            
                </v-card-title>

                <v-divider></v-divider>
                <v-card-text>
                    <v-flex xs12  class="pa-3" v-if="format === 'png'">
                        <v-img :src="grahpURL" alt=""></v-img>
                    </v-flex>
                    <v-flex xs12 class="pa-3" v-if="format === 'json' && selectedMbService.value === 'seasonAverages'">                       
                        <p>
                            <span class="title" style="color: #37aa48; font-size 12px;">
                                {{selectedMbService.name}}
                            </span>
                        </p>
                        <p> {{seasonAveragesText}} </p>

                        <img style="width: 750px;" src="../assets/seasonAveragesTable.png" alt="">
                    </v-flex>   
                    <v-flex xs12 class="pa-3" v-if="format === 'json' && selectedMbService.value === 'yearlyAverages'">                       
                        <p>
                            <span class="title" style="color: #37aa48; font-size 12px;">
                                {{selectedMbService.name}}
                            </span>
                        </p>
                        <p> {{yearlyAveragesText}} </p>
                        <img style="width: 750px;" src="../assets/yearlyAveragesTable.png" alt="">
                        <p class="mt-3">
                            <span class="title" style="font-size 12px;">
                                Temperatures
                            </span>
                        </p>
                        <p> The API returns min/max/mean of yearly values of min, max or mean temperature relative to the two costumisable time periods. </p>

                        <img style="width: 750px;" src="../assets/yearlyAveragesTemperaturesTable.png" alt="">
                    </v-flex>     

                    <v-flex xs12 class="pa-3" v-if="format === 'json' && selectedMbService.value === 'eventDailyDistributions'">                       
                        <p>
                            <span class="title" style="color: #37aa48; font-size 12px;">
                                {{selectedMbService.name}}
                            </span>
                        </p>
                        <p> {{eventDailyDistributionsText}} </p>

                        <img style="width: 750px;" src="../assets/eventDailyDistributionsTable.png" alt="">
                    </v-flex>
                    <v-flex xs12 class="pa-3" v-if="format === 'json' && selectedMbService.value === 'eventHourlyDistributions'">                       
                        <p>
                            <span class="title" style="color: #37aa48; font-size 12px;">
                                {{selectedMbService.name}}
                            </span>
                        </p>
                        <p> {{eventHourlyDistributionsText}} </p>

                        <img style="width: 750px;" src="../assets/eventDailyDistributionsTable.png" alt="">
                    </v-flex>                       

                    <v-flex xs12  class="pa-3" v-if="format === 'highchartsHtml'"> 
                        <iframe 
                            width="100%"
                            height="600"
                            frameborder="0" style="overflow:hidden;"
                            :src="grahpURL">
                        </iframe> 
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
        format: "",
        outputjson: {},
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
        cropClimateText: 'The Crop climate risk comparison diagrams show the comparison of the main weather variables (temperature, precipitation,' +
            'evapotranspiration, radiation) averaged on customisable time periods. Based on more than 30 years of historical simulation data' +
            'for any location on earth.',
        seasonAveragesText: 'The Season averages diagram shows the comparison, along a customisable growing season, of average curves of the selected weather' +
            'variable relative to two different time periods (typically the last 10 years vs 10 years of the past). The distributions compare' +
            'the Kernel Density Estimation for the two selected time periods.',
        yearlyAveragesText: 'The Yearly averages diagram shows the comparison of the yearly values of the selected weather variable relative to two' +
            'costumisable time periods (typically the last 10 years vs 10 years of the past). The distributions compare the Kernel Density' +
            'Estimation for the two selected time periods.',
        eventDailyDistributionsText: 'The API returns event series meaningful at daily level (e.g. daily maximum temperature above a certain threshold), relative to' +
            'two costumizable time periods (typically the last 10 years vs 10 years of the past). The diagram compares the Kernel Density' +
            'Estimation and statistical distributions for the two selected time periods.',
        eventHourlyDistributionsText: 'The API returns event series meaningful at hourly level (e.g. 1 hour of precipitation above a certain threshold) in 3 possible' +
            'aggregations (number of events/season, number of hours/season, number of days/season), relative to two costumizable time periods' +
            '(typically the last 10 years vs 10 years of the past). The diagram compares the Kernel Density Estimation and statistical' +
            'distributions for the two selected time periods.',
    }),
    methods: {   
        getOutput(url, format){
                
            this.outputDialog = true;
            this.grahpURL = url;
            this.format = format;
            var self = this;           

            if(format === 'json'){
                this.$http.get(url).then(response => {                             
                    this.outputjson = response.body;    
                    this.isLoading = false;         
                    self.$eventBus.$emit('show-alert', "success", "JSON retrieved successfully");                           
                }, response => {
                    this.isLoading = false;                    
                    this.$eventBus.$emit('show-alert', "error", response.statusText); 
                });

            }else{
                setTimeout(function(){                                                                    
                    self.$eventBus.$emit('show-alert', "success", "Diagram retrieved successfully");   
                    self.isLoading = false;                  
                }, 4000); 
            }                                                          
        },
        /**
        * Open the image in other browser tab to force to be download
        *
        * @public
        */
        downloadOutput(format){
            if(format === 'png'){
                window.open(this.grahpURL)  
            }else{
                var data = JSON.stringify(this.outputjson)
                var blob = new Blob([data], {type: 'text/plain'})
                var e = document.createEvent('MouseEvents'),
                a = document.createElement('a');
                a.download = "output.json";
                a.href = window.URL.createObjectURL(blob);
                a.dataset.downloadurl = ['text/json', a.download, a.href].join(':');
                e.initEvent('click', true, false, window, 0, 0, 0, 0, 0, false, false, false, false, 0, null);
                a.dispatchEvent(e);
            }          
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
	
.wrapper {
    position: relative;
    width: 80%;
    height: 0;
    padding-bottom: 75%;
    overflow: hidden;
}
.wrapper iframe {
    position: absolute;
    top:0;
    left: 0;
    width: 100%;
    height: 100%;
}

</style>


