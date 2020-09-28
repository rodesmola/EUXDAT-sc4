<template>
  
    <div>        
        <v-flex xs12 pl-2 row class="hidden-md-and-down">
            <v-layout row wrap>
            <p style="color: #27304c; font-size 6px;" class="pl-2 pr-2">
                Select an analisys to run for the area you are seeing in the map. 
                Then click the "run" button to display the result.
                <a @click="infoDialog = true">More info.</a>
            </p>
            </v-layout>
        </v-flex>                    

        <v-flex xs12 pl-2 row>
            <v-layout row wrap class="pl-2 pr-2"> 

                <v-flex sm9 xs6 md6 lg6 xlg6 class="pl-3 pr-3">
                    <v-combobox style="margin-top: 0px; padding-top: 0px"
                    v-model="eventRiskSelected"
                    :items="eventRiskArr"
                    item-text="name"
                    item-value="value"
                    label="Select risk event"
                    color="green"
                    ></v-combobox>
                </v-flex>

                <v-flex sm9 xs6 md6 lg6 xlg6 class="pl-3 pr-3">
                    <v-combobox style="margin-top: 0px; padding-top: 0px"
                    v-model="cropOfInterestSelected"
                    :items="cropOfInterestArr"
                    item-text="name"
                    item-value="value"
                    label="Select crop"
                    color="green"
                    ></v-combobox>
                </v-flex>

                <v-flex xs8 sm8 md6 lg8 xlg8 class="pl-3 pr-3">
                    <v-text-field dense color="#77b942" type="text" v-model="location_name" :value="location_name"
                    label="Diagram title"></v-text-field>
                </v-flex>

                <v-flex sm9 xs6 md6 lg4 xlg4 class="pl-3 pr-3">
                    <v-combobox 
                    v-model="formatSelected"
                    :items="formatArr"
                    item-text="name"
                    item-value="value"
                    label="Select Diagram format"
                    color="green"
                    ></v-combobox>
                </v-flex>

                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field dense color="#77b942" type="number" v-model="years_start" :value="years_start"
                    label="Start year" :rules="inputNumRules"></v-text-field>
                </v-flex>
                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field dense color="#77b942" type="number" v-model="years_end" :value="years_end"
                    label="End year" :rules="inputNumRules"></v-text-field>
                </v-flex>
                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field dense color="#77b942" type="number" v-model="month_start" :value="month_start"
                    label="Start month" :rules="inputNumRules"></v-text-field>
                </v-flex>
                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field dense color="#77b942" type="number" v-model="month_end" :value="month_end"
                    label="End month" :rules="inputNumRules"></v-text-field>
                </v-flex>
                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field dense color="#77b942" type="number" v-model="t_base" :value="t_base"
                    label="Base temp" :rules="inputNumRules"></v-text-field>
                </v-flex>
                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field dense color="#77b942" type="number" v-model="t_max" :value="t_max"
                    label="Max temp" :rules="inputNumRules"></v-text-field>
                </v-flex>
                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field dense color="#77b942" type="number" v-model="drought_threshold" :value="drought_threshold"
                    label="Drought threshold" :rules="inputNumRules"></v-text-field>
                </v-flex>
                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field dense color="#77b942" type="number" v-model="drought_duration" :value="drought_duration"
                    label="Drought duration" :rules="inputNumRules"></v-text-field>
                </v-flex>
                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field dense color="#77b942" type="number" v-model="frost_threshold" :value="frost_threshold"
                    label="Frost threshold" :rules="inputNumRules"></v-text-field>
                </v-flex>
                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field dense color="#77b942" type="number" v-model="frost_duration" :value="frost_duration"
                    label="Frost duration" :rules="inputNumRules"></v-text-field>
                </v-flex>
                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field dense color="#77b942" type="number" v-model="soil_water_capacity" :value="soil_water_capacity"
                    label="Soil water capacity" :rules="inputNumRules"></v-text-field>
                </v-flex>

            </v-layout>
        </v-flex>
                    
        <v-flex xs12 sm12 md12 lg12 class="text-xs-right" style="padding: 0px; margin-bottom: 5px;">
            <v-btn small round color="#27304c" :loading="isLoading" dark @click="runService()" title="Run service" >
                RUN
            </v-btn>
        </v-flex>
        <!------------ Info dialog ------------>
        <v-dialog v-model="infoDialog" max-width="800">
            <v-card>
                <v-card-title class="headline">
                    <img style="width: 155px;" src="../assets/logo_titulo.png" alt="">
                </v-card-title>
                <v-divider></v-divider>
                <v-card-text>
                    <v-flex xs12>
                        <v-card flat>
                            <v-card-text>
                                <div style="margin-bottom: 15px">
                                    <img style="width: 160px; position: absolute; opacity: 0.2; bottom: 20px; right: 5px;" src="../assets/logo2.png" alt="">
                                    <span class="title" style="color: #37aa48; font-size 12px;">Crop climate risk analysis</span>
                                </div>                    
                                <p>The Crop Climate Risk diagram shows the risk of occurrence of a weather event (drought, frost), in the selected location, 
                                for a specific crop, during its growing season.</p>
                                <p>The risk is calculated as the product of likelihood of the event (based on historical statistics for the selected time 
                                 period) and the impact on the crop (growing phase specific, based on empirical/literature assessment).</p>
                                <p>Crop growing phases, based on cumulated growing degree days (GDD), are plotted in the background.</p>
                            </v-card-text>
                        </v-card>
                    </v-flex>                           
                </v-card-text>
                <v-divider></v-divider>
                <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="green darken-1" dark small @click="infoDialog = false">
                    Got it!
                </v-btn>
                </v-card-actions>
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
                    <v-flex xs12  class="pa-3" v-if="formatSelected='png'">
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

// import moment from 'moment';
export default {
    name: "RiskAnalysis",
    props: {},
    data: () => ({
        isValid: false,
        inputNumRules: [            
            v => (v && /^\d+(\.\d{1,20})?$/.test(v)) || ''
        ],  
        API_key: "1a98a8ef2598-EU-SG-testing",
        eventRiskSelected: 'Drought',
        eventRiskArr: ['Drought', 'Frost'],
        cropOfInterestSelected: 'Cotton',
        cropOfInterestArr: ['Cotton', 'Maize', 'Soybeans', 'Spring_Barley', 'Summer_Wheat', 'Sunflowers', 'Vineyards', 'Winter_Wheat'],
        formatSelected: 'png',
        formatArr: ['png', 'json', 'highchartsHtml'],
        years_start: 2010,
        years_end: 2020,
        month_start: "",
        month_end: "",
        t_base: "",
        t_max: "",
        soil_water_capacity: "",
        drought_threshold: "",
        drought_duration: 3,
        frost_threshold: 0,
        frost_duration: 3,
        location_name: '',
        isLoading: false,
        outputDialog: false,   
        infoDialog: false, 
        grahpURL: ""     
    }),
    methods: {
         /**
        * Create the url to acces the output mb diagram
        *        
        * @public
        */
        runService(){

            var self = this;
            this.isLoading = true; 
            var coords4326 = this.$store.state.map.getView().getCenter();           

            this.grahpURL = "http://pyapi.meteoblue.com/cropClimateRisk/".concat(
                this.eventRiskSelected + '/' + 
                this.cropOfInterestSelected + '/' + 
                coords4326[1].toString() + '/' + 
                coords4326[0].toString() + '/' + 
                this.API_key + '?' +
                'format=' + this.formatSelected +
                '&years_start=' + this.years_start +
                '&years_end=' + this.years_end +
                '&drought_duration=' + this.drought_duration +
                '&frost_threshold=' + this.frost_threshold +
                '&frost_duration=' + this.frost_duration
            )

            if(this.month_start){
                this.grahpURL = this.grahpURL.concat('&month_start=', this.month_start)
            }
            if(this.month_end){    
                this.grahpURL = this.grahpURL.concat('&month_end=', this.month_end)
            }
            if(this.t_base){    
                this.grahpURL = this.grahpURL.concat('&T_base=', this.t_base)
            }
            if(this.t_max){    
                this.grahpURL = this.grahpURL.concat('&T_max=', this.t_max)
            }
            if(this.soil_water_capacity){    
                this.grahpURL = this.grahpURL.concat('&soil_water_capacity=', this.soil_water_capacity)
            }
            if(this.drought_threshold){    
                this.grahpURL = this.grahpURL.concat('&drought_threshold=', this.drought_threshold)
            }
            if(this.location_name){    
                this.grahpURL = this.grahpURL.concat('&location_name=', this.location_name)
            }

this.grahpURL = 'http://pyapi.meteoblue.com/cropClimateRisk/Drought/Maize/49.1/16.6/1a98a8ef2598-EU-SG-testing?format=png&years_start=2010&years_end=2020'
            this.$http.get(this.grahpURL).then(response => {
                                    
                setTimeout(function(){ 
                    self.isLoading = false;                                                 
                    self.$eventBus.$emit('show-alert', "success", "Diagram retrieved successfully"); 
                    self.outputDialog = true;
                }, 4000);                           
            }, response => {
                this.isLoading = false;                    
                this.$eventBus.$emit('show-alert', "error", response.statusText); 
            });

            console.log(this.grahpURL)


        }        
    },
    filters: {
        truncate: function(value) {
            if(value != undefined){
                value = value.toString().substring(0, 8);
            }
            return value
        }
    }

};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>


