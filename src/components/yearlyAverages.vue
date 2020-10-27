<template>

    <v-form ref="yearlyAveragesForm" v-model="yearlyAveragesValid">
        <v-layout row wrap>

            <v-flex xs8 sm8 md6 lg8 xlg8 class="pl-3 pr-3">
                <v-text-field hide-no-data hide-selected dense color="#77b942" type="text" v-model="yearlyAverages.location_name" :value="yearlyAverages.location_name"
                label="Diagram title"></v-text-field>
            </v-flex>

            <v-flex sm9 xs6 md6 lg4 xlg4 class="pl-3 pr-3">
                <v-combobox hide-no-data hide-selected dense
                v-model="yearlyAverages.selectedFormat"
                :items="yearlyAverages.formatArr"
                item-text="name"
                item-value="value"
                label="Select diagram format"
                color="green"
                ></v-combobox>
            </v-flex>

            <v-flex sm9 xs6 md6 lg6 xlg6 class="pl-3 pr-3">
                <v-combobox hide-no-data hide-selected dense
                v-model="yearlyAverages.selectedType"
                :items="yearlyAverages.typeArr"
                item-text="name"
                item-value="value"
                label="Select output diagram type"
                color="green"
                ></v-combobox>
            </v-flex>
            <v-flex sm9 xs6 md6 lg6 xlg6 class="pl-3 pr-3">
                <v-combobox hide-no-data hide-selected dense
                v-model="yearlyAverages.selectedName"
                :items="yearlyAverages.nameArr"
                item-text="name"
                item-value="value"
                label="Variable of interest"
                color="green"
                ></v-combobox>
            </v-flex>

            <v-flex sm9 xs6 md6 lg6 xlg6 class="pl-3 pr-3" v-if="yearlyAverages.selectedName === 'Temperature'">
                <v-combobox hide-no-data hide-selected dense
                v-model="yearlyAverages.selectedAggr"
                :items="yearlyAverages.aggregationArr"
                item-text="name"
                item-value="value"
                label="Variable of interest"
                color="green"
                ></v-combobox>
            </v-flex>                
            <v-flex xs3 class="pl-3 pr-3">
                <v-text-field hide-no-data hide-selected dense color="#77b942" type="text" v-model="yearlyAverages.years_clima_start" 
                    :value="yearlyAverages.years_clima_start" label="First year comp." title="First year of past comparison period, from 1985 onwards." 
                    @input="$v.yearlyAverages.years_clima_start.$touch()" @blur="$v.yearlyAverages.years_clima_start.$touch()"
                    :error-messages="years_clima_startErrors"> 
                </v-text-field>
            </v-flex>  
            <v-flex xs3 class="pl-3 pr-3">
                <v-text-field hide-no-data hide-selected dense color="#77b942" type="text" v-model="yearlyAverages.years_clima_end" 
                    :value="yearlyAverages.years_clima_end" label="Last year comp." title="Last year of past comparison period." 
                    @input="$v.yearlyAverages.years_clima_end.$touch()" @blur="$v.yearlyAverages.years_clima_end.$touch()"
                    :error-messages="years_clima_endErrors">
                </v-text-field>
            </v-flex>   
            <v-flex xs3 class="pl-3 pr-3">
                <v-text-field hide-no-data hide-selected dense color="#77b942" type="text" v-model="yearlyAverages.years_actual_start" 
                    :value="yearlyAverages.years_actual_start" label="First year of recent comp." title="First year of recent comparison period, from 1985 onwards."
                    @input="$v.yearlyAverages.years_actual_start.$touch()" @blur="$v.yearlyAverages.years_actual_start.$touch()"
                    :error-messages="years_actual_startErrors">
                </v-text-field>
            </v-flex>            
            <v-flex xs3 class="pl-3 pr-3">
                <v-text-field hide-no-data hide-selected dense color="#77b942" type="text" v-model="yearlyAverages.years_actual_end" 
                    :value="yearlyAverages.years_actual_end" label="Last year of recent comp." title="Last year of recent comparison period." 
                    @input="$v.yearlyAverages.years_actual_end.$touch()" @blur="$v.yearlyAverages.years_actual_end.$touch()"
                    :error-messages="years_actual_endErrors">
                </v-text-field>                    
            </v-flex>   
            <v-flex xs6 sm6 md6 lg6 class="text-xs-right mt-3 pr-2" style="padding: 0px; margin-bottom: 5px;" v-if="yearlyAverages.selectedName === 'Temperature'">
                <v-btn small round color="#27304c" :disabled="!yearlyAveragesValid" :loading="isLoading" dark @click="runService()" title="Run service" >
                RUN
                </v-btn>
            </v-flex> 
          <v-flex xs12 class="text-xs-right pr-2" style="padding: 0px; margin-bottom: 5px;" v-if="yearlyAverages.selectedName != 'Temperature'">
                <v-btn small round color="#27304c" :disabled="!yearlyAveragesValid" :loading="isLoading" dark @click="runService()" title="Run service" >
                RUN
                </v-btn>
            </v-flex>             
        </v-layout>
    </v-form>       

</template>

<script>
import {between, numeric} from 'vuelidate/lib/validators';
import CONST from "../const";
export default {
    name: "yearlyAverages",    
    data: () => ({       
        baseAPIurl: CONST.baseAPIurl,        
        API_key: CONST.API_key,   
        isLoading: false,        
        yearlyAveragesValid: false,                                 
        yearlyAverages: {
            location_name: '',
            nameArr: ['Temperature', 'Precipitation', 'Radiation', 'Evapotranspiration'],
            selectedName: "Temperature",
            aggregationArr: ['min', 'max', 'mean'],
            selectedAggr: 'mean',
            threshold: "",
            duration: "",
            typeArr: ['Diagram', 'Distribution'],
            selectedType: 'Diagram',
            selectedFormat: 'png',
            formatArr: ['png', 'json', 'highchartsHtml'],
            years_clima_start: "",
            years_clima_end: "",
            years_actual_start: "",
            years_actual_end: ""
        },

    }),
    methods: {   
        runService(){
            this.$v.$touch()
            
            var url;
  
            if(this.yearlyAverages.selectedName === 'Temperature'){
                url = this.baseAPIurl.concat('yearlyAveragesTemp/', this.yearlyAverages.selectedName, '/', this.$store.state.mapCoords.lat, '/', this.$store.state.mapCoords.long, 
                '/', this.yearlyAverages.selectedAggr, '/')
            }else{
                url = this.baseAPIurl.concat('yearlyAveragesOther/', this.yearlyAverages.selectedName, '/', this.$store.state.mapCoords.lat, '/', this.$store.state.mapCoords.long, '/')        
            }

            url = url.concat(this.API_key, '?format=', this.yearlyAverages.selectedFormat, '&type=', this.yearlyAverages.selectedType);

            if(this.yearlyAverages.years_clima_start){
                url = url.concat('&years_clima_start=', this.yearlyAverages.years_clima_start)
            }
            if(this.yearlyAverages.years_clima_end){
                url = url.concat('&years_clima_end=', this.yearlyAverages.years_clima_end)
            }
            if(this.yearlyAverages.years_actual_start){
                url = url.concat('&years_actual_start=', this.yearlyAverages.years_actual_start)
            }
            if(this.yearlyAverages.years_actual_end){
                url = url.concat('&years_actual_end=', this.yearlyAverages.years_actual_end)
            }
            if(this.yearlyAverages.location_name){
                url = url.concat('&location_name=', this.yearlyAverages.location_name)
            }
            
            this.$eventBus.$emit('get-output', url, this.yearlyAverages.selectedFormat); 
            
        }
    },    
    validations: {
       yearlyAverages:{       
            years_clima_start: {between: between(1985, 2020), numeric},  
            years_clima_end: {between: between(1985, 2020), numeric},  
            years_actual_start: {between: between(1985, 2020), numeric},
            years_actual_end: {between: between(1985, 2020), numeric},  
        }
    },
    computed: {
        month_startErrors () {
            const errors = []
            if (!this.$v.yearlyAverages.month_start.$dirty) return errors
            !this.$v.yearlyAverages.month_start.required && errors.push('Required field.')
            !this.$v.yearlyAverages.month_start.between && errors.push('Values from 0 to 12')
            !this.$v.yearlyAverages.month_start.numeric && errors.push('Insert a number')
            return errors
        },
        month_endErrors () {
            const errors = []
            if (!this.$v.yearlyAverages.month_end.$dirty) return errors
            !this.$v.yearlyAverages.month_end.required && errors.push('Required field.')
            !this.$v.yearlyAverages.month_end.between && errors.push('Values from 0 to 12')
            !this.$v.yearlyAverages.month_end.numeric && errors.push('Insert a number')
            return errors
        },
        years_clima_startErrors () {
            const errors = []
            if (!this.$v.yearlyAverages.years_clima_start.$dirty) return errors
            !this.$v.yearlyAverages.years_clima_start.between && errors.push('Values from 1985 to 2020')
            !this.$v.yearlyAverages.years_clima_start.numeric && errors.push('Insert a number')
            return errors
        },     
        years_clima_endErrors () {
            const errors = []
            if (!this.$v.yearlyAverages.years_clima_end.$dirty) return errors
            !this.$v.yearlyAverages.years_clima_end.between && errors.push('Values from 1985 to 2020')
            !this.$v.yearlyAverages.years_clima_end.numeric && errors.push('Insert a number')
            return errors
        },      
        years_actual_startErrors () {
            const errors = []
            if (!this.$v.yearlyAverages.years_actual_start.$dirty) return errors
            !this.$v.yearlyAverages.years_actual_start.between && errors.push('Values from 1985 to 2020')
            !this.$v.yearlyAverages.years_actual_start.numeric && errors.push('Insert a number')
            return errors
        },    
        years_actual_endErrors () {
            const errors = []
            if (!this.$v.yearlyAverages.years_actual_end.$dirty) return errors
            !this.$v.yearlyAverages.years_actual_end.between && errors.push('Values from 1985 to 2020')
            !this.$v.yearlyAverages.years_actual_end.numeric && errors.push('Insert a number')
            return errors
        },                  
    }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>