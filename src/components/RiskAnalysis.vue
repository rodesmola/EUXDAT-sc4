<template>
  
    <div>        
        <v-flex xs12 pl-2 row class="hidden-md-and-down">
            <v-layout row wrap>
            <p style="color: #27304c; font-size 6px;" class="pl-3 pr-3">
                The service will use the coordinates showed in the rigth side of the map, can be input by the user.                 
                <a @click="infoDialog = true">More info.</a>
            </p>
            </v-layout>
        </v-flex>                    

        <v-flex xs12 pl-2 row>
            <v-form ref="riskAnalysisForm" v-model="riskAnalysisValid">
                <v-layout row wrap class="pl-2 pr-2"> 

                    <v-flex sm9 xs6 md6 lg6 xlg6 class="pl-3 pr-3">
                        <v-combobox hide-no-data hide-selected dense style="margin-top: 0px; padding-top: 0px"
                        v-model="riskAnalysis.eventRiskSelected"
                        :items="riskAnalysis.eventRiskArr"
                        item-text="name"
                        item-value="value"
                        label="Select risk event"
                        color="green"
                        ></v-combobox>
                    </v-flex>

                    <v-flex sm9 xs6 md6 lg6 xlg6 class="pl-3 pr-3">
                        <v-combobox hide-no-data hide-selected dense style="margin-top: 0px; padding-top: 0px"
                        v-model="riskAnalysis.cropOfInterestSelected"
                        :items="riskAnalysis.cropOfInterestArr"
                        item-text="name"
                        item-value="value"
                        label="Select crop"
                        color="green"
                        ></v-combobox>
                    </v-flex>

                    <v-flex xs8 sm8 md6 lg8 xlg8 class="pl-3 pr-3">
                        <v-text-field hide-no-data hide-selected dense color="#77b942" type="text" v-model="riskAnalysis.location_name" :value="riskAnalysis.location_name"
                        label="Diagram title"></v-text-field>
                    </v-flex>

                    <v-flex sm9 xs6 md6 lg4 xlg4 class="pl-3 pr-3">
                        <v-combobox hide-no-data hide-selected dense
                        v-model="riskAnalysis.formatSelected"
                        :items="riskAnalysis.formatArr"
                        item-text="name"
                        item-value="value"
                        label="Select Diagram format"
                        color="green"
                        ></v-combobox>
                    </v-flex>

                    <v-flex xs3 class="pl-3 pr-3">
                        <v-text-field hide-no-data hide-selected dense color="#77b942" type="text" v-model="riskAnalysis.years_start" 
                            :value="riskAnalysis.years_start" label="Start year" title="First year of period of interest, from 1985 onwards." 
                            @input="$v.riskAnalysis.years_start.$touch()" @blur="$v.riskAnalysis.years_start.$touch()"
                            :error-messages="years_startErrors">
                        </v-text-field>
                    </v-flex>
                    <v-flex xs3 class="pl-3 pr-3">
                        <v-text-field hide-no-data hide-selected dense color="#77b942" type="text" v-model="riskAnalysis.years_end" 
                            :value="riskAnalysis.years_end" label="End year" title="Last year of period of interest."
                            @input="$v.riskAnalysis.years_end.$touch()" @blur="$v.riskAnalysis.years_end.$touch()"
                            :error-messages="years_endErrors">
                        </v-text-field>                        
                    </v-flex>
                    <v-flex xs3 class="pl-3 pr-3">
                        <v-text-field hide-no-data hide-selected dense color="#77b942" type="text" v-model="riskAnalysis.month_start" 
                            :value="riskAnalysis.month_start" label="Start month" title="Initial month of crop typical growing season." 
                            @input="$v.riskAnalysis.month_start.$touch()" @blur="$v.riskAnalysis.month_start.$touch()"
                            :error-messages="month_startErrors">
                        </v-text-field>                         
                    </v-flex>
                    <v-flex xs3 class="pl-3 pr-3">
                        <v-text-field hide-no-data hide-selected dense color="#77b942" type="text" v-model="riskAnalysis.month_end" 
                            :value="riskAnalysis.month_end" label="End month" title="Final month of crop typical growing season."
                            @input="$v.riskAnalysis.month_end.$touch()" @blur="$v.riskAnalysis.month_end.$touch()"
                            :error-messages="month_endErrors">
                        </v-text-field>                            
                    </v-flex>
                    <v-flex xs3 class="pl-3 pr-3">
                        <v-text-field hide-no-data hide-selected dense color="#77b942" type="text" v-model="riskAnalysis.t_base" 
                            :value="riskAnalysis.t_base" label="Base temp." title="Crop base temperature (°C)."
                            @input="$v.riskAnalysis.t_base.$touch()" @blur="$v.riskAnalysis.t_base.$touch()"
                            :error-messages="t_baseErrors">
                        </v-text-field>   
                    </v-flex>
                    <v-flex xs3 class="pl-3 pr-3">
                        <v-text-field hide-no-data hide-selected dense color="#77b942" type="text" v-model="riskAnalysis.t_max" 
                            :value="riskAnalysis.t_max" label="Max temp." title="Crop max temperature (°C)."
                            @input="$v.riskAnalysis.t_max.$touch()" @blur="$v.riskAnalysis.t_max.$touch()"
                            :error-messages="t_maxErrors">
                        </v-text-field>  
                    </v-flex>
                    <v-flex xs3 class="pl-3 pr-3">
                        <v-text-field hide-no-data hide-selected dense color="#77b942" type="text" v-model="riskAnalysis.drought_threshold" 
                            :value="riskAnalysis.drought_threshold" label="Drought threshold" title="Soil water content threshold (mm) below which drought happens." 
                            @input="$v.riskAnalysis.drought_threshold.$touch()" @blur="$v.riskAnalysis.drought_threshold.$touch()"
                            :error-messages="drought_thresholdErrors">
                        </v-text-field> 
                    </v-flex>
                    <v-flex xs3 class="pl-3 pr-3">
                        <v-text-field hide-no-data hide-selected dense color="#77b942" type="text" v-model="riskAnalysis.drought_duration" 
                            :value="riskAnalysis.drought_duration"  label="Drought duration"  title="Duration threshold (hours) above which drought happens."
                            @input="$v.riskAnalysis.drought_duration.$touch()" @blur="$v.riskAnalysis.drought_duration.$touch()"
                            :error-messages="drought_durationErrors">
                        </v-text-field> 
                    </v-flex>
                    <v-flex xs3 class="pl-3 pr-3">
                        <v-text-field hide-no-data hide-selected dense color="#77b942" type="text" v-model="riskAnalysis.frost_threshold" 
                            :value="riskAnalysis.frost_threshold" label="Frost threshold" title="Temperature threshold (°C) below which frost happens."
                            @input="$v.riskAnalysis.frost_threshold.$touch()" @blur="$v.riskAnalysis.frost_threshold.$touch()"
                            :error-messages="frost_thresholdErrors">
                        </v-text-field> 
                    </v-flex>
                    <v-flex xs3 class="pl-3 pr-3">
                        <v-text-field hide-no-data hide-selected dense color="#77b942" type="text" v-model="riskAnalysis.frost_duration" 
                            :value="riskAnalysis.frost_duration" label="Frost duration"  title="Duration threshold (hours) above which frost happens."
                            @input="$v.riskAnalysis.frost_duration.$touch()" @blur="$v.riskAnalysis.frost_duration.$touch()"
                            :error-messages="frost_durationErrors">
                        </v-text-field> 
                    </v-flex>
                    <v-flex xs3 class="pl-3 pr-3">
                        <v-text-field hide-no-data hide-selected dense color="#77b942" type="text" v-model="riskAnalysis.soil_water_capacity" 
                            :value="riskAnalysis.soil_water_capacity" label="Soil water capacity" title="Maximum possible soil water content (mm)."
                            @input="$v.riskAnalysis.soil_water_capacity.$touch()" @blur="$v.riskAnalysis.soil_water_capacity.$touch()"
                            :error-messages="soil_water_capacityErrors">
                        </v-text-field> 
                    </v-flex>

                    <v-flex xs12 sm12 md12 lg12 class="text-xs-right pr-2" style="padding: 0px; margin-bottom: 5px;">
                        <v-btn small round color="#27304c" :disabled="!riskAnalysisValid" :loading="isLoading" dark @click="runService()" title="Run service" >
                        RUN
                        </v-btn>
                    </v-flex>

                </v-layout>
            </v-form> 
        </v-flex>
                    

        <!------------ Info dialog ------------>
        <v-dialog v-model="infoDialog" max-width="900">
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
                                <img style="width: 850px" src="../assets/crop_climate_risk1.png" alt="">                               
                                <img style="width: 850px" src="../assets/crop_climate_risk2.png" alt="">
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
        <v-dialog v-model="outputDialog" max-width="900">
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

                    <v-flex xs12 class="pa-3" v-if="format === 'json'">                       
                        <p>
                            <span class="title" style="color: #37aa48; font-size 12px;">
                                Crop climate risk analysis
                            </span>
                        </p>
                        <p> 'The Crop Climate Risk diagram shows the risk of occurrence of a weather event (drought, frost), in the selected location, 
                            for a specific crop, during its growing season. The risk is calculated as the product of likelihood of the event 
                            (based on historical statistics for the selected time period) and the impact on the crop (growing phase specific, 
                            based on empirical/literature assessment). Crop growing phases, based on cumulated growing degree days (GDD), 
                            are plotted in the background.' 
                        </p>

                        <img style="width: 750px;" src="../assets/riskAnalysisTable.png" alt="">
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

import { decimal, between, numeric } from 'vuelidate/lib/validators'
export default {
    name: "RiskAnalysis",    
    data: () => ({
        riskAnalysisValid: false,
        API_key: "1a98a8ef2598-EU-SG-testing",
        isLoading: false,
        outputDialog: false,   
        infoDialog: false, 
        format: "",
        grahpURL: "",
        riskAnalysis: {
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
        }    
    }),
    validations: {
        riskAnalysis:{       
            month_start: {between: between(0, 12), numeric},        
            month_end: {between: between(0, 12), numeric},             
            t_base: {decimal},
            t_max: {decimal},
            years_start: {between: between(1985, 2020), numeric},
            years_end: {between: between(1985, 2020), numeric},              
            soil_water_capacity: {numeric},
            drought_threshold: {numeric},
            drought_duration: {numeric},
            frost_threshold: {numeric},
            frost_duration: {numeric},          
        }
    },
    computed: {
        month_startErrors () {
            const errors = []
            if (!this.$v.riskAnalysis.month_start.$dirty) return errors
            !this.$v.riskAnalysis.month_start.between && errors.push('Values from 0 to 12')
            !this.$v.riskAnalysis.month_start.numeric && errors.push('Insert a number')
            return errors
        },
        month_endErrors () {
            const errors = []
            if (!this.$v.riskAnalysis.month_end.$dirty) return errors
            !this.$v.riskAnalysis.month_end.between && errors.push('Values from 0 to 12')
            !this.$v.riskAnalysis.month_end.numeric && errors.push('Insert a number')
            return errors
        },
        t_baseErrors () {
            const errors = []
            if (!this.$v.riskAnalysis.t_base.$dirty) return errors
            !this.$v.riskAnalysis.t_base.decimal && errors.push('Insert a number')
            return errors
        },
        t_maxErrors () {
            const errors = []
            if (!this.$v.riskAnalysis.t_max.$dirty) return errors
            !this.$v.riskAnalysis.t_max.decimal && errors.push('Insert a number')
            return errors
        },  
        years_startErrors () {
            const errors = []
            if (!this.$v.riskAnalysis.years_start.$dirty) return errors
            !this.$v.riskAnalysis.years_start.between && errors.push('Values from 1985 to 2020')
            !this.$v.riskAnalysis.years_start.numeric && errors.push('Insert a number')
            return errors
        },     
        years_endErrors () {
            const errors = []
            if (!this.$v.riskAnalysis.years_end.$dirty) return errors
            !this.$v.riskAnalysis.years_end.between && errors.push('Values from 1985 to 2020')
            !this.$v.riskAnalysis.years_end.numeric && errors.push('Insert a number')
            return errors
        },  
        soil_water_capacityErrors () {
            const errors = []
            if (!this.$v.riskAnalysis.t_max.$dirty) return errors
            !this.$v.riskAnalysis.t_max.decimal && errors.push('Insert a number')
            return errors
        },   
        drought_thresholdErrors () {
            const errors = []
            if (!this.$v.riskAnalysis.t_max.$dirty) return errors
            !this.$v.riskAnalysis.t_max.decimal && errors.push('Insert a number')
            return errors
        }, 
        drought_durationErrors () {
            const errors = []
            if (!this.$v.riskAnalysis.t_max.$dirty) return errors
            !this.$v.riskAnalysis.t_max.decimal && errors.push('Insert a number')
            return errors
        },    
        frost_thresholdErrors () {
            const errors = []
            if (!this.$v.riskAnalysis.t_max.$dirty) return errors
            !this.$v.riskAnalysis.t_max.decimal && errors.push('Insert a number')
            return errors
        }, 
        frost_durationErrors () {
            const errors = []
            if (!this.$v.riskAnalysis.t_max.$dirty) return errors
            !this.$v.riskAnalysis.t_max.decimal && errors.push('Insert a number')
            return errors
        },              
    },

    methods: {
         /**
        * Create the url to acces the output mb diagram
        *        
        * @public
        */
        runService(){
            
            this.isLoading = true; 
                       
            var url = "http://pyapi.meteoblue.com/cropClimateRisk/".concat(
                this.riskAnalysis.eventRiskSelected + '/' + 
                this.riskAnalysis.cropOfInterestSelected + '/' + 
                this.$store.state.mapCoords.lat + '/' + 
                this.$store.state.mapCoords.long + '/' + 
                this.API_key + '?' +
                'format=' + this.riskAnalysis.formatSelected +
                '&years_start=' + this.riskAnalysis.years_start +
                '&years_end=' + this.riskAnalysis.years_end +
                '&drought_duration=' + this.riskAnalysis.drought_duration +
                '&frost_threshold=' + this.riskAnalysis.frost_threshold +
                '&frost_duration=' + this.riskAnalysis.frost_duration
            )

            if(this.riskAnalysis.month_start){
                url = url.concat('&month_start=', this.riskAnalysis.month_start)
            }
            if(this.riskAnalysis.month_end){    
                url = url.concat('&month_end=', this.riskAnalysis.month_end)
            }
            if(this.riskAnalysis.t_base){    
                url = url.concat('&T_base=', this.riskAnalysis.t_base)
            }
            if(this.riskAnalysis.t_max){    
                url = url.concat('&T_max=', this.riskAnalysis.t_max)
            }
            if(this.riskAnalysis.soil_water_capacity){    
                url = url.concat('&soil_water_capacity=', this.riskAnalysis.soil_water_capacity)
            }
            if(this.riskAnalysis.drought_threshold){    
                url = url.concat('&drought_threshold=', this.riskAnalysis.drought_threshold)
            }
            if(this.riskAnalysis.location_name){    
                url = url.concat('&location_name=', this.riskAnalysis.location_name)
            }           

           this.getOutput(url, this.riskAnalysis.formatSelected);
        },
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
    filters: {
        truncate: function(value) {
            if(value != undefined){
                value = value.toString().substring(0, 8);
            }
            return value
        }
    },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>


