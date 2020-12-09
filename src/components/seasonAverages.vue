<template>

    <v-form ref="seasonAveragesForm" v-model="seasonAveragesValid">
        <v-layout row wrap>

            <v-flex xs8 sm8 md6 lg8 xlg8 class="pl-3 pr-3">
                <v-text-field hide-no-data hide-selected dense color="#77b942" type="text" v-model="seasonAverages.location_name" :value="seasonAverages.location_name"
                label="Location name" title="Location name, labels the diagram, has no effect on data."></v-text-field>
            </v-flex>

            <v-flex sm9 xs6 md6 lg4 xlg4 class="pl-3 pr-3">
                <v-combobox  hide-no-data hide-selected dense 
                v-model="seasonAverages.selectedFormat"
                :items="seasonAverages.formatArr"
                item-text="name"
                item-value="value"
                label="Select diagram format"
                color="green"
                ></v-combobox>
            </v-flex>

            <v-flex sm9 xs6 md6 lg6 xlg6 class="pl-3 pr-3">
                <v-combobox hide-no-data hide-selected dense 
                v-model="seasonAverages.selectedType"
                :items="seasonAverages.typeArr"
                item-text="name"
                item-value="value"
                label="Select output diagram type"
                color="green"
                ></v-combobox>
            </v-flex>

            <v-flex sm9 xs6 md6 lg6 xlg6 class="pl-3 pr-3">
                <v-combobox hide-no-data hide-selected dense 
                v-model="seasonAverages.selectedVariableOfInt"
                :items="seasonAverages.variableOfInterestArr"
                item-text="name"
                item-value="value"
                label="Select variable of interest"
                color="green"
                ></v-combobox>
            </v-flex>

            <v-flex sm9 xs6 md6 lg6 xlg6 class="pl-3 pr-3">
                <v-combobox hide-no-data hide-selected dense 
                v-model="seasonAverages.selectedAggr"
                :items="seasonAverages.aggrArr"
                item-text="name"
                item-value="value"
                label="Select variable of interest"
                color="green"
                ></v-combobox>
            </v-flex>

            <v-flex xs3 class="pl-3 pr-3">
                <v-text-field  dense color="#77b942" hide-no-data hide-selected  type="text" v-model="seasonAverages.month_start" 
                    :value="seasonAverages.month_start" label="Initial month *" title="Initial month of growing season" 
                    @input="$v.seasonAverages.month_start.$touch()" @blur="$v.seasonAverages.month_start.$touch()"
                    :error-messages="month_startErrors">
                </v-text-field>
            </v-flex>
            <v-flex xs3 class="pl-3 pr-3">
                <v-text-field  dense color="#77b942" hide-no-data hide-selected  type="text" v-model="seasonAverages.month_end" 
                    :value="seasonAverages.month_end" label="Final month *" title="Final month of growing season" 
                    @input="$v.seasonAverages.month_end.$touch()" @blur="$v.seasonAverages.month_end.$touch()"
                    :error-messages="month_endErrors">
                </v-text-field>
            </v-flex>
            <v-flex xs3 class="pl-3 pr-3">
                <v-text-field hide-no-data hide-selected dense color="#77b942" type="text" v-model="seasonAverages.t_base" 
                    :value="seasonAverages.t_base" label="Base temp" title="Crop base temperature (°C)."
                    @input="$v.seasonAverages.t_base.$touch()" @blur="$v.seasonAverages.t_base.$touch()"
                    :error-messages="t_baseErrors">                        
                </v-text-field>
            </v-flex>
            <v-flex xs3 class="pl-3 pr-3">
                <v-text-field hide-no-data hide-selected dense color="#77b942" type="text" v-model="seasonAverages.t_max" 
                    :value="seasonAverages.t_max" label="Max temp" title="Crop max temperature (°C)."
                    @input="$v.seasonAverages.t_max.$touch()" @blur="$v.seasonAverages.t_max.$touch()"
                    :error-messages="t_maxErrors">                        
                </v-text-field>
            </v-flex>
            <v-flex xs3 class="pl-3 pr-3">
                <v-text-field hide-no-data hide-selected dense color="#77b942" type="text" v-model="seasonAverages.years_clima_start" 
                    :value="seasonAverages.years_clima_start" label="First year comp." title="First year of past comparison period, from 1985 onwards."
                    @input="$v.seasonAverages.years_clima_start.$touch()" @blur="$v.seasonAverages.years_clima_start.$touch()"
                    :error-messages="years_clima_startErrors">                        
                </v-text-field>
            </v-flex>
            <v-flex xs3 class="pl-3 pr-3">
                <v-text-field hide-no-data hide-selected dense color="#77b942" type="text" v-model="seasonAverages.years_clima_end" 
                    :value="seasonAverages.years_clima_end" label="Last year comp." title="Last year of past comparison period."
                    @input="$v.seasonAverages.years_clima_end.$touch()" @blur="$v.seasonAverages.years_clima_end.$touch()"
                    :error-messages="years_clima_endErrors">                        
                </v-text-field>
            </v-flex>

            <v-flex xs3 class="pl-3 pr-3">
                <v-text-field hide-no-data hide-selected dense color="#77b942" type="text" v-model="seasonAverages.years_actual_start" 
                    :value="seasonAverages.years_actual_start" label="First year of recent comp." title="First year of recent comparison period, from 1985 onwards." 
                    @input="$v.seasonAverages.years_actual_start.$touch()" @blur="$v.seasonAverages.years_actual_start.$touch()"
                    :error-messages="years_actual_startErrors">  
                </v-text-field>
            </v-flex>  

            <v-flex xs3 class="pl-3 pr-3">
                <v-text-field hide-no-data hide-selected dense color="#77b942" type="text" v-model="seasonAverages.years_actual_end" 
                    :value="seasonAverages.years_actual_end" label="Last year of recent comp." title="Last year of recent comparison period."
                    @input="$v.seasonAverages.years_actual_end.$touch()" @blur="$v.seasonAverages.years_actual_end.$touch()"
                    :error-messages="years_actual_endErrors">
                </v-text-field>
            </v-flex>  

            <v-flex xs6 sm6 md6 lg6 class="text-xs-right mt-3 pr-2" style="padding: 0px; margin-bottom: 5px;">
                <v-btn small round color="#27304c" :disabled="!seasonAveragesValid" :loading="isLoading" dark @click="runService()" title="Run service" >
                RUN
                </v-btn>
            </v-flex>

        </v-layout>
    </v-form> 
       

</template>

<script>
import { required, between, numeric, decimal } from 'vuelidate/lib/validators';
import CONST from "../const";
export default {
    name: "seasonAverages",    
    data: () => ({      
        baseAPIurl: CONST.baseAPIurl,        
        API_key: CONST.API_key,             
        isLoading: false,
        seasonAveragesValid: false,                                 
        seasonAverages:{
            variableOfInterestArr: ['GDD', 'Precipitation', 'Radiation', 'Evapotranspiration'],
            selectedVariableOfInt:'GDD',
            month_start: 5,
            month_end: 10,
            selectedFormat: 'png',
            formatArr: ['png', 'highchartsHtml'], // ['png', 'json', 'highchartsHtml'],
            location_name: '',
            t_base: 3,
            t_max: 30,
            typeArr: ['Diagram', 'Distribution'],
            selectedType: 'Diagram',
            aggrArr: ['Cumulated', 'Daily'],
            selectedAggr: 'Cumulated',
            years_clima_start:"",
            years_clima_end: "",
            years_actual_start: "",
            years_actual_end: ""
        }

    }),
    methods: {   
        runService(){
            this.$v.$touch()           

            var url = this.baseAPIurl.concat('seasonAverages/', this.seasonAverages.selectedVariableOfInt, '/', this.$store.state.mapCoords.lat, '/', this.$store.state.mapCoords.long, 
                '/', this.seasonAverages.month_start, '/', this.seasonAverages.month_end, '/', this.API_key, '?format=', this.seasonAverages.selectedFormat,
                '&type=', this.seasonAverages.selectedType, '&t_base=', this.seasonAverages.t_base, '&t_max=', this.seasonAverages.t_max);

            if(this.seasonAverages.years_clima_start){
                url = url.concat('&years_clima_start=', this.seasonAverages.years_clima_start);
            }
            if(this.seasonAverages.years_clima_end){
                url = url.concat('&years_clima_end=', this.seasonAverages.years_clima_end);
            }
            if(this.seasonAverages.years_actual_start){
                url = url.concat('&years_actual_start=', this.seasonAverages.years_actual_start);
            }
            if(this.seasonAverages.years_actual_end){
                url = url.concat('&years_actual_end=', this.seasonAverages.years_actual_end);
            }
            if(this.seasonAverages.location_name){
                url = url.concat('&location_name=', this.seasonAverages.location_name);
            }             
            this.$eventBus.$emit('get-output', url, this.seasonAverages.selectedFormat);         

        }
    },    
    validations: {
       seasonAverages:{       
            month_start: {required, between: between(0, 12), numeric},
            month_end: {required, between: between(0, 12), numeric},                   
            t_base: {decimal},
            t_max: {decimal},
            years_clima_start: {between: between(1985, 2020), numeric},  
            years_clima_end: {between: between(1985, 2020), numeric},  
            years_actual_start: {between: between(1985, 2020), numeric},
            years_actual_end: {between: between(1985, 2020), numeric},  
        }
    },
    computed: {
        month_startErrors () {
            const errors = []
            if (!this.$v.seasonAverages.month_start.$dirty) return errors
            !this.$v.seasonAverages.month_start.required && errors.push('Required field.')
            !this.$v.seasonAverages.month_start.between && errors.push('Values from 0 to 12')
            !this.$v.seasonAverages.month_start.numeric && errors.push('Insert a number')
            return errors
        },
        month_endErrors () {
            const errors = []
            if (!this.$v.seasonAverages.month_end.$dirty) return errors
            !this.$v.seasonAverages.month_end.required && errors.push('Required field.')
            !this.$v.seasonAverages.month_end.between && errors.push('Values from 0 to 12')
            !this.$v.seasonAverages.month_end.numeric && errors.push('Insert a number')
            return errors
        },
        t_baseErrors () {
            const errors = []
            if (!this.$v.seasonAverages.t_base.$dirty) return errors
            !this.$v.seasonAverages.t_base.decimal && errors.push('Insert a number')
            return errors
        },
        t_maxErrors () {
            const errors = []
            if (!this.$v.seasonAverages.t_max.$dirty) return errors
            !this.$v.seasonAverages.t_max.decimal && errors.push('Insert a number')
            return errors
        },   
        years_clima_startErrors () {
            const errors = []
            if (!this.$v.seasonAverages.years_clima_start.$dirty) return errors
            !this.$v.seasonAverages.years_clima_start.between && errors.push('Values from 1985 to 2020')
            !this.$v.seasonAverages.years_clima_start.numeric && errors.push('Insert a number')
            return errors
        },     
        years_clima_endErrors () {
            const errors = []
            if (!this.$v.seasonAverages.years_clima_end.$dirty) return errors
            !this.$v.seasonAverages.years_clima_end.between && errors.push('Values from 1985 to 2020')
            !this.$v.seasonAverages.years_clima_end.numeric && errors.push('Insert a number')
            return errors
        },      
        years_actual_startErrors () {
            const errors = []
            if (!this.$v.seasonAverages.years_actual_start.$dirty) return errors
            !this.$v.seasonAverages.years_actual_start.between && errors.push('Values from 1985 to 2020')
            !this.$v.seasonAverages.years_actual_start.numeric && errors.push('Insert a number')
            return errors
        },    
        years_actual_endErrors () {
            const errors = []
            if (!this.$v.seasonAverages.years_actual_end.$dirty) return errors
            !this.$v.seasonAverages.years_actual_end.between && errors.push('Values from 1985 to 2020')
            !this.$v.seasonAverages.years_actual_end.numeric && errors.push('Insert a number')
            return errors
        },                  
    },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>