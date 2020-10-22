<template>

    <v-form ref="eventDailyDistributionsForm" v-model="eventDailyDistributionsValid">
        <v-layout row wrap>

            <v-flex xs8 sm8 md6 lg8 xlg8 class="pl-3 pr-3">
                <v-text-field hide-no-data hide-selected dense color="#77b942" type="text" v-model="eventDailyDistributions.location_name" :value="eventDailyDistributions.location_name"
                label="Diagram title"></v-text-field>
            </v-flex>

            <v-flex sm9 xs6 md6 lg4 xlg4 class="pl-3 pr-3">
                <v-combobox hide-no-data hide-selected dense
                v-model="eventDailyDistributions.selectedFormat"
                :items="eventDailyDistributions.formatArr"
                item-text="name"
                item-value="value"
                label="Select diagram format"
                color="green"
                ></v-combobox>
            </v-flex>
            <v-flex sm9 xs6 md6 lg6 xlg6 class="pl-3 pr-3">
                <v-combobox hide-no-data hide-selected dense
                v-model="eventDailyDistributions.selectedEvent"
                :items="eventDailyDistributions.event_aggrArr"
                item-text="name"
                item-value="value"
                label="Aggregation of event"
                color="green"
                ></v-combobox>
            </v-flex>

            <v-flex sm9 xs6 md6 lg6 xlg6 class="pl-3 pr-3">
                <v-combobox hide-no-data hide-selected dense
                v-model="eventDailyDistributions.selectedName"
                :items="eventDailyDistributions.nameArr"
                item-text="name"
                item-value="value"
                label="Variable of interest"
                color="green"
                ></v-combobox>
            </v-flex>

            <v-flex xs3 class="pl-3 pr-3">
                <v-text-field hide-selected dense color="#77b942" type="number" v-model="eventDailyDistributions.month_start" 
                    :value="eventDailyDistributions.month_start" label="Initial month" title="Initial month of growing season" 
                    @input="$v.eventDailyDistributions.month_start.$touch()" @blur="$v.eventDailyDistributions.month_start.$touch()"
                    :error-messages="month_startErrors">
                </v-text-field>
            </v-flex>
            <v-flex xs3 class="pl-3 pr-3">
                <v-text-field hide-no-data hide-selected dense color="#77b942" type="number" v-model="eventDailyDistributions.month_end" 
                    :value="eventDailyDistributions.month_end" label="Final month" title="Final month of growing season" 
                    @input="$v.eventDailyDistributions.month_end.$touch()" @blur="$v.eventDailyDistributions.month_end.$touch()"
                    :error-messages="month_endErrors">
                </v-text-field>
            </v-flex>

            <v-flex xs3 class="pl-3 pr-3">
                <v-text-field hide-no-data hide-selected dense color="#77b942" type="number" v-model="eventDailyDistributions.years_clima_start" 
                    :value="eventDailyDistributions.years_clima_start" label="First year comp." title="First year of past comparison period, from 1985 onwards." 
                    @input="$v.eventDailyDistributions.years_clima_start.$touch()" @blur="$v.eventDailyDistributions.years_clima_start.$touch()"
                    :error-messages="years_clima_startErrors">                        
                </v-text-field>
            </v-flex>  
            <v-flex xs3 class="pl-3 pr-3">
                <v-text-field hide-no-data hide-selected dense color="#77b942" type="number" v-model="eventDailyDistributions.years_clima_end" 
                    :value="eventDailyDistributions.years_clima_end" label="Last year comp." title="Last year of past comparison period." 
                    @input="$v.eventDailyDistributions.years_clima_end.$touch()" @blur="$v.eventDailyDistributions.years_clima_end.$touch()"
                    :error-messages="years_clima_endErrors">                        
                </v-text-field>
            </v-flex>   
            <v-flex xs3 class="pl-3 pr-3">
                <v-text-field hide-no-data hide-selected dense color="#77b942" type="number" v-model="eventDailyDistributions.years_actual_start" 
                    :value="eventDailyDistributions.years_actual_start" label="First year of recent comp." title="First year of recent comparison period, from 1985 onwards." 
                    @input="$v.eventDailyDistributions.years_actual_start.$touch()" @blur="$v.eventDailyDistributions.years_actual_start.$touch()"
                    :error-messages="years_actual_startErrors">  
                </v-text-field>
            </v-flex>            
            <v-flex xs3 class="pl-3 pr-3">
                <v-text-field hide-no-data hide-selected dense color="#77b942" type="number" v-model="eventDailyDistributions.years_actual_end" 
                    :value="eventDailyDistributions.years_actual_end" label="Last year of recent comp." title="Last year of recent comparison period." 
                    @input="$v.eventDailyDistributions.years_actual_end.$touch()" @blur="$v.eventDailyDistributions.years_actual_end.$touch()"
                    :error-messages="years_actual_endErrors">
                </v-text-field>
            </v-flex>    

            <v-flex xs3 class="pl-3 pr-3">
                <v-text-field hide-no-data hide-selected dense color="#77b942" type="number" v-model="eventDailyDistributions.threshold" 
                    :value="eventDailyDistributions.threshold" label="Threshold" title="Threshold below/above which event happens." 
                    @input="$v.eventDailyDistributions.threshold.$touch()" @blur="$v.eventDailyDistributions.threshold.$touch()"
                    :error-messages="thresholdErrors">
                </v-text-field>
            </v-flex>    

            <v-flex xs3 class="pl-3 pr-3">
                <v-text-field hide-no-data hide-selected dense color="#77b942" type="number" v-model="eventDailyDistributions.duration" 
                    :value="eventDailyDistributions.duration" label="Duration" title="Duration threshold (hours) above which event happens." 
                    @input="$v.eventDailyDistributions.duration.$touch()" @blur="$v.eventDailyDistributions.duration.$touch()"
                    :error-messages="durationErrors">
                </v-text-field>
            </v-flex>

            <v-flex xs3 class="pl-3 pr-3">
                <v-text-field hide-no-data hide-selected dense color="#77b942" type="number" v-model="eventDailyDistributions.var_maximum" 
                    :value="eventDailyDistributions.var_maximum" label="Maximum possible variable value" title="Maximum possible variable value (when meaningful)." 
                    @input="$v.eventDailyDistributions.var_maximum.$touch()" @blur="$v.eventDailyDistributions.var_maximum.$touch()"
                    :error-messages="var_maximumErrors">
                </v-text-field>
            </v-flex>
            <v-flex xs9 sm9 md9 lg9 class="text-xs-right mt-3 pr-2" style="padding: 0px; margin-bottom: 5px;">
                <v-btn small round color="#27304c" :disabled="!eventDailyDistributionsValid" :loading="isLoading" dark @click="runService()" title="Run service" >
                RUN
                </v-btn>
            </v-flex>
        </v-layout>
    </v-form> 
       

</template>

<script>
import { required, between, numeric } from 'vuelidate/lib/validators'
export default {
    name: "eventDailyDistributions",    
    data: () => ({        
        isLoading: false,
        API_key: "1a98a8ef2598-EU-SG-testing", 
        eventDailyDistributionsValid: false,                                 
                eventDailyDistributions: {
            location_name: '',
            nameArr: ['Frost', 'Icing', 'Heat waves', 'Precipitation', 'Drought', 'Tropical nights'],
            selectedName: "Precipitation",
            selectedFormat: 'png',
            formatArr: ['png', 'json', 'highchartsHtml'],
            month_start: 5,
            month_end: 10,
            years_clima_start:"",
            years_clima_end: "",
            years_actual_start: "",
            years_actual_end: "",
            threshold: "",
            duration: "",
            var_maximum: "",
            event_aggrArr: [
                {
                    name: 'Number of events/season',
                    value: 'number%20of%20events%20%2F%20season'
                }
            ],
            selectedEvent:{
                name: 'Number of events/season',
                value: 'number%20of%20events%20%2F%20season'
            },     
        },
    }),
    validations: {
       eventDailyDistributions:{   
            threshold: {numeric},
            duration: {between: between(0, 24), numeric},
            var_maximum: {numeric},
            month_start: {required, between: between(0, 12), numeric},
            month_end: {required, between: between(0, 12), numeric},                   
            years_clima_start: {between: between(1985, 2020), numeric},  
            years_clima_end: {between: between(1985, 2020), numeric},  
            years_actual_start: {between: between(1985, 2020), numeric},
            years_actual_end: {between: between(1985, 2020), numeric},  
        }
    },
    computed: {
        thresholdErrors () {
            const errors = []
            if (!this.$v.eventDailyDistributions.month_start.$dirty) return errors
            !this.$v.eventDailyDistributions.month_start.numeric && errors.push('Insert a number')
            return errors
        },        
        durationErrors () {
            const errors = []
            if (!this.$v.eventDailyDistributions.month_start.$dirty) return errors
            !this.$v.eventDailyDistributions.month_start.numeric && errors.push('Insert a number')
            !this.$v.eventDailyDistributions.month_start.between && errors.push('Values from 0 to 24')
            return errors
        },   
        var_maximumErrors () {
            const errors = []
            if (!this.$v.eventDailyDistributions.month_start.$dirty) return errors
            !this.$v.eventDailyDistributions.month_start.numeric && errors.push('Insert a number')
            return errors
        },           
        month_startErrors () {
            const errors = []
            if (!this.$v.eventDailyDistributions.month_start.$dirty) return errors
            !this.$v.eventDailyDistributions.month_start.required && errors.push('Required field.')
            !this.$v.eventDailyDistributions.month_start.between && errors.push('Values from 0 to 12')
            !this.$v.eventDailyDistributions.month_start.numeric && errors.push('Insert a number')
            return errors
        },
        month_endErrors () {
            const errors = []
            if (!this.$v.eventDailyDistributions.month_end.$dirty) return errors
            !this.$v.eventDailyDistributions.month_end.required && errors.push('Required field.')
            !this.$v.eventDailyDistributions.month_end.between && errors.push('Values from 0 to 12')
            !this.$v.eventDailyDistributions.month_end.numeric && errors.push('Insert a number')
            return errors
        },
        years_clima_startErrors () {
            const errors = []
            if (!this.$v.eventDailyDistributions.years_clima_start.$dirty) return errors
            !this.$v.eventDailyDistributions.years_clima_start.between && errors.push('Values from 1985 to 2020')
            !this.$v.eventDailyDistributions.years_clima_start.numeric && errors.push('Insert a number')
            return errors
        },     
        years_clima_endErrors () {
            const errors = []
            if (!this.$v.eventDailyDistributions.years_clima_end.$dirty) return errors
            !this.$v.eventDailyDistributions.years_clima_end.between && errors.push('Values from 1985 to 2020')
            !this.$v.eventDailyDistributions.years_clima_end.numeric && errors.push('Insert a number')
            return errors
        },      
        years_actual_startErrors () {
            const errors = []
            if (!this.$v.eventDailyDistributions.years_actual_start.$dirty) return errors
            !this.$v.eventDailyDistributions.years_actual_start.between && errors.push('Values from 1985 to 2020')
            !this.$v.eventDailyDistributions.years_actual_start.numeric && errors.push('Insert a number')
            return errors
        },    
        years_actual_endErrors () {
            const errors = []
            if (!this.$v.eventDailyDistributions.years_actual_end.$dirty) return errors
            !this.$v.eventDailyDistributions.years_actual_end.between && errors.push('Values from 1985 to 2020')
            !this.$v.eventDailyDistributions.years_actual_end.numeric && errors.push('Insert a number')
            return errors
        },                  
    },
    methods: {   
        runService(){
            this.$v.$touch()
            var url = 'http://pyapi.meteoblue.com/';

            if(this.$refs.eventDailyDistributionsForm.validate()){        
        
                url = url.concat('eventDailyDistributions/', this.eventDailyDistributions.selectedName, '/', this.$store.state.mapCoords.lat, 
                    '/', this.$store.state.mapCoords.long, '/', this.eventDailyDistributions.month_start, '/', this.eventDailyDistributions.month_end, '/', 
                    this.API_key, '?format=', this.eventDailyDistributions.selectedFormat, '&event_aggr=', this.eventDailyDistributions.selectedEvent.value);

                if(this.eventDailyDistributions.years_clima_start){
                    url = url.concat('&years_clima_start=', this.eventDailyDistributions.years_clima_start)
                }
                if(this.eventDailyDistributions.years_clima_end){
                    url = url.concat('&years_clima_end=', this.eventDailyDistributions.years_clima_end)
                }
                if(this.eventDailyDistributions.years_actual_start){
                    url = url.concat('&years_actual_start=', this.eventDailyDistributions.years_actual_start)
                }
                if(this.eventDailyDistributions.years_actual_end){
                    url = url.concat('&years_actual_end=', this.eventDailyDistributions.years_actual_end)
                }
                if(this.eventDailyDistributions.location_name){
                    url = url.concat('&location_name=', this.eventDailyDistributions.location_name)
                }
                if(this.eventDailyDistributions.threshold){
                    url = url.concat('&threshold=', this.eventDailyDistributions.threshold)
                }
                if(this.eventDailyDistributions.duration){
                    url = url.concat('&duration=', this.eventDailyDistributions.duration)
                }
                if(this.eventDailyDistributions.var_maximum){
                    url = url.concat('&var_maximum=', this.eventDailyDistributions.var_maximum)
                }              
                
                this.$eventBus.$emit('get-output', url, this.eventDailyDistributions.selectedFormat); 
            }

        }
    },

};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>