<template>
<div>  
    <div>        
        <v-flex xs12 pl-2 row class="hidden-md-and-down">
            <v-layout row wrap>
            <p style="color: #27304c; font-size 6px;" class="pl-2 pr-2">
                Select an analisys to run for the coordinates showed on top of the map. Then click the "run" button to display the result. 
            </p>
            </v-layout>
        </v-flex>                    

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

        <v-spacer></v-spacer>

        <v-flex xs12 pl-2 row v-if="selectedMbService.value == 'history_frostrisk?'">
            <v-layout row wrap>
                <v-flex xs12 class="pl-3 pr-3">
                    <p style="color: grey; font-size 8px; margin-bottom: 5px;">
                        <b>Cold event analysis:</b>
                        Probability of temperatures below or above a certain temperature threshold for a given time.
                    </p>
                </v-flex>
                <v-flex xs6 class="pl-3 pr-3">
                    <v-text-field hide-details hide-no-data hide-selected dense color="#77b942" type="number" v-model="cold_tmp_threshold" :value="cold_tmp_threshold"
                    label="Temperature threshold *" :rules="inputNumRules" required></v-text-field>
                </v-flex>
                <v-flex xs6 class="pl-3 pr-3">
                    <v-text-field hide-details hide-no-data hide-selected dense color="#77b942" type="number" v-model="cold_dur_threshold" :value="cold_dur_threshold"
                    label="Duration threshold *" :rules="inputNumRules" required></v-text-field>
                </v-flex>
            </v-layout>
        </v-flex>

        <v-flex xs12 pl-2 row v-if="selectedMbService.value == 'history_heatrisk?'">
            <v-layout row wrap>
                <v-flex xs12 class="pl-3 pr-3">
                    <p style="color: grey; font-size 8px;">
                        <b>Warm event analysis:</b>
                        Probability of temperatures below or above a certain temperature threshold for a given time.
                    </p>
                </v-flex>
                <v-flex xs6 class="pl-3 pr-3">
                    <v-text-field hide-details hide-no-data hide-selected dense color="#77b942" type="number" v-model="warn_tmp_threshold" :value="warn_tmp_threshold"
                    label="Temperature threshold *" :rules="inputNumRules" required></v-text-field>
                </v-flex>
                <v-flex xs6 class="pl-3 pr-3">
                    <v-text-field hide-details hide-no-data hide-selected dense color="#77b942" type="number" v-model="warn_dur_threshold" :value="warn_dur_threshold"
                    label="Duration threshold *" :rules="inputNumRules" required></v-text-field>
                </v-flex>
            </v-layout>
        </v-flex>

        <v-flex xs12 pl-2 row v-if="selectedMbService.value == 'history_preciprisk?'">
            <v-layout row wrap>
                <v-flex xs12 class="pl-3 pr-3">
                    <p style="color: grey; font-size 8px;">
                        <b>Precipitation analysis:</b>
                        evaluates precipitation amounts in a week above a certain threshold. Similar to cold and warm events,
                        estimate strong precipitation events can be estimated for scheduling activities accordingly.
                    </p>
                </v-flex>
                <v-flex xs6 class="pl-3 pr-3">
                    <v-text-field hide-details hide-no-data hide-selected dense color="#77b942" type="number" v-model="prec_threshold" :value="prec_threshold"
                    label="Precipitation threshold *" :rules="inputNumRules" required></v-text-field>
                </v-flex>
            </v-layout>
        </v-flex>

        <v-flex xs12 pl-2 row  v-if="selectedMbService.value == 'history_watercapacity?'">
            <v-layout row wrap>
                <v-flex xs12 class="pl-3 pr-3">
                    <p style="color: grey; font-size 8px;">
                        <b>Water capacity:</b>
                        estimated probability of remaining soil water amount. Select the maximum amount of soil water capacity depending on your soil and crop type.
                    </p>
                </v-flex>
                <v-flex xs6 class="pl-3 pr-3">
                    <v-text-field hide-details hide-no-data hide-selected dense color="#77b942" type="number" v-model="wat_cap_threshold" :value="wat_cap_threshold"
                    label="Maximum available soil water capacity threshold *" :rules="inputNumRules" required></v-text-field>
                </v-flex>
            </v-layout>
        </v-flex>

        <v-flex xs12 pl-2 row  v-if="selectedMbService.value == 'history_cloudcover?'">
            <v-layout row wrap>
                <v-flex xs12 class="pl-3 pr-3">
                    <p style="color: grey; font-size 8px;">
                        <b>Cloud cover:</b>
                        cloud free hours per day.
                    </p>
                </v-flex>
                <v-flex xs6 class="pl-3 pr-3">
                    <v-text-field hide-no-data hide-selected dense color="#77b942" type="number" v-model="cloud_cvr_threshold" :value="cloud_cvr_threshold"
                    label="Cloud cover threshold % *" :rules="inputNumRules" required></v-text-field>
                </v-flex>
            </v-layout>
        </v-flex>

                    
        <v-flex xs12 sm12 md12 lg12 class="text-xs-right mt-2" style="padding: 0px; margin-bottom: 5px;">
            <v-btn small round color="#27304c" :disabled="!selectedMbService" :loading="isLoading" dark @click="mbService(selectedMbService)" title="Run service" >
            RUN
            </v-btn>
        </v-flex>
    </div>

    <!------------ MeteoBlue dialog ------------>
    <v-dialog v-model="mbDialog" max-width="800">
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
    <!------------ /MeteoBlue dialog ------------>

    </div>

</template>

<script>

import moment from 'moment';
export default {
    name: "ClimaticPatterns",
    props: {},
    data: () => ({
        isValid: false,
        inputNumRules: [
        //v => !!v || 'Required field',
        v => (v && /^\d+(\.\d{1,20})?$/.test(v)) || ''
        ],
        cold_tmp_threshold: 1,
        cold_dur_threshold: 1,
        warn_tmp_threshold: 30,
        warn_dur_threshold: 1,
        prec_threshold: 30,
        wat_cap_threshold: 100,
        cloud_cvr_threshold: 15,
        API_key: "8vh83gfhu34g",
        mbServices: [
        {
            'name': 'Cold event analysis',
            'value': 'history_frostrisk?'
        },
        {
            'name': 'Warm event analysis',
            'value': 'history_heatrisk?'
        },
        {
            'name': 'Precipitation history analysis',
            'value': 'history_preciprisk?'
        },
        {
            'name': 'Soil water capacity analysis',
            'value': 'history_watercapacity?'
        },
        {
            'name': 'Cloud cover analysis',
            'value': 'history_cloudcover?'
        }
        ],
        selectedMbService:"",
        grahpURL: "",
        mbDialog: false,
        isLoading: false,
    }),
    methods: {
        /**
        * Create the url to acces the output mb diagram
        *
        * @param {string} service
        * @public
        */
        mbService(service){

            var self = this;
            this.isLoading = true;            
            var coords4326 = this.$store.state.map.getView().getCenter();           

            var url = 'http://my.meteoblue.com/visimage/'.concat(service.value,
            '&lat=', coords4326[1].toString(), '&lon=', coords4326[0].toString(), '&apikey=', this.API_key);

            if(service.value === 'history_frostrisk?'){
                this.grahpURL = url.concat('&thr=', this.cold_tmp_threshold, '&frostlength=', this.cold_dur_threshold);
            }else if (service.value === 'history_heatrisk?') {
                this.grahpURL = url.concat('&thr=', this.warn_tmp_threshold, '&frostlength=', this.warn_dur_threshold);
            }else if (service.value === 'history_preciprisk?') {
                this.grahpURL = url.concat('&thr=', this.prec_threshold);
            }else if (service.value === 'history_watercapacity?') {
                this.grahpURL = url.concat('&thr=', this.wat_cap_threshold);
            }else if (service.value === 'history_cloudcover?') {
                this.grahpURL = url.concat('&thr=', this.cloud_cvr_threshold);
            }

            this.$http.get(this.graphURL).then(response => {
                                    
                setTimeout(function(){ 
                    self.isLoading = false;                                                 
                    self.$eventBus.$emit('show-alert', "success", "Diagram retrieved successfully"); 
                    self.mbDialog = true;
                }, 4000);                           
            }, response => {
                this.isLoading = false;                    
                this.$eventBus.$emit('show-alert', "error", response.statusText); 
            });
        
        },//mbService
        /**
        * Open the image in other browser tab to force to be download
        *
        * @public
        */
        downloadGraph(){
            window.open(this.grahpURL)  
        },
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


