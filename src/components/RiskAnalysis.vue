<template>
  
    <div>        
        <v-flex xs12 pl-2 row class="hidden-md-and-down">
            <v-layout row wrap>
            <p style="color: #27304c; font-size 6px;" class="pl-2 pr-2">
                Select an analisys to run for the area you are seeing in the map. 
                Then click the "run" button to display the result.
            </p>
            </v-layout>
        </v-flex>                    

        <v-flex xs12 pl-2 row>
            <v-layout row wrap>
                RiskAnalysis
            </v-layout>
        </v-flex>
                    
        <v-flex xs12 sm12 md12 lg12 class="text-xs-right" style="padding: 0px; margin-bottom: 5px;">
            <!-- <v-btn small round color="#27304c" :disabled="!selectedMbService" :loading="isLoading" dark @click="mbService(selectedMbService)" title="Run service" >
            RUN
            </v-btn> -->
        </v-flex>
    </div>

</template>

<script>

import moment from 'moment';
export default {
    name: "RiskAnalysis",
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
        gs_cropName: ['Maize', 'Sunflower', 'Summer_wheat'],
        gs_wDomain: ['NEMSGLOBAL', 'NEMS4'],
        growth_stages:{
        planting_date: '2015-04-14',
        stem_elong_date: '2015-05-10',
        crop_name: 'Maize',
        T_base: 10,
        T_max: 30,
        w_domain: 'NEMSGLOBAL',
        },

        selectedMbService:"",
        grahpURL: "",
        mbDialog: false,
        mbGSDialog: false,
        diagramURL: {},
        outputPanel: false,
        isLoading: false,
        inputDateRules: [
        v => !!v || ''
        ],
        menuPlantingDate: false,
        menuStemDate: false,
    }),
    watch: {
        menuPlantingDate (val) {
            val && setTimeout(() => (this.$refs.picker.activePicker = 'YEAR'))
        },
        menuStemDate (val) {
            val && setTimeout(() => (this.$refs.picker.activePicker = 'YEAR'))
        },  
    },
    methods: {
   
        savePlantingDate (date) {
            this.$refs.menuPlantingDate.save(date)
        },
        saveStemDate (date) {
            this.$refs.menuStemDate.save(date)
        },
    },
    created(){
    }, 
    filters: {
        truncate: function(value) {
            if(value != undefined){
                value = value.toString().substring(0, 8);
            }
            return value
        },
        formatDate: function(value) {
            if (value) {
                return moment(String(value)).format('MM/DD/YYYY hh:mm')
            }
        }
    }

};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>


