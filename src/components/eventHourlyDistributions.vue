<template>

    <v-form ref="eventHourlyDistributionsForm" v-model="eventHourlyDistributionsValid">
        <v-layout row wrap>

            <v-flex xs8 sm8 md6 lg8 xlg8 class="pl-3 pr-3">
                <v-text-field hide-no-data hide-selected dense color="#77b942" type="text" v-model="eventHourlyDistributions.location_name" :value="eventHourlyDistributions.location_name"
                label="Diagram title"></v-text-field>
            </v-flex>
            <v-flex sm9 xs6 md6 lg4 xlg4 class="pl-3 pr-3">
                <v-combobox hide-no-data hide-selected dense
                v-model="eventHourlyDistributions.selectedFormat"
                :items="eventHourlyDistributions.formatArr"
                item-text="name"
                item-value="value"
                label="Select diagram format"
                color="green"
                ></v-combobox>
            </v-flex>
            <v-flex sm9 xs6 md6 lg6 xlg6 class="pl-3 pr-3">
                <v-combobox hide-no-data hide-selected dense
                v-model="eventHourlyDistributions.selectedName"
                :items="eventHourlyDistributions.nameArr"
                item-text="name"
                item-value="value"
                label="Variable of interest"
                color="green"
                ></v-combobox>
            </v-flex>
            <v-flex sm9 xs6 md6 lg6 xlg6 class="pl-3 pr-3">
                <v-combobox hide-no-data hide-selected dense
                v-model="eventHourlyDistributions.selectedEvent"
                :items="eventHourlyDistributions.event_aggrArr"
                item-text="name"
                item-value="value"
                label="Aggregation of event"
                color="green"
                ></v-combobox>
            </v-flex>

            <v-flex xs3 class="pl-3 pr-3">
                <v-text-field hide-no-data hide-selected dense color="#77b942" type="number" v-model="eventHourlyDistributions.month_start" 
                    :value="eventHourlyDistributions.month_start" label="Initial month" title="Initial month of growing season" 
                    @input="$v.eventHourlyDistributions.month_start.$touch()" @blur="$v.eventHourlyDistributions.month_start.$touch()"
                    :error-messages="month_startErrors">
                </v-text-field>
            </v-flex>
            <v-flex xs3 class="pl-3 pr-3">
                <v-text-field hide-no-data hide-selected dense color="#77b942" type="number" v-model="eventHourlyDistributions.month_end" 
                    :value="eventHourlyDistributions.month_end" label="Final month" title="Final month of growing season" 
                    @input="$v.eventHourlyDistributions.month_end.$touch()" @blur="$v.eventHourlyDistributions.month_end.$touch()"
                    :error-messages="month_endErrors">
                </v-text-field>
            </v-flex>
            <v-flex xs3 class="pl-3 pr-3">
                <v-text-field hide-no-data hide-selected dense color="#77b942" type="number" v-model="eventHourlyDistributions.years_clima_start" 
                    :value="eventHourlyDistributions.years_clima_start" label="First year comp." title="First year of past comparison period, from 1985 onwards." 
                    @input="$v.eventHourlyDistributions.years_clima_start.$touch()" @blur="$v.eventHourlyDistributions.years_clima_start.$touch()"
                    :error-messages="years_clima_startErrors">                        
                </v-text-field>
            </v-flex>  
            <v-flex xs3 class="pl-3 pr-3">
                <v-text-field hide-no-data hide-selected dense color="#77b942" type="number" v-model="eventHourlyDistributions.years_clima_end" 
                    :value="eventHourlyDistributions.years_clima_end" label="Last year comp." title="Last year of past comparison period." 
                    @input="$v.eventHourlyDistributions.years_clima_end.$touch()" @blur="$v.eventHourlyDistributions.years_clima_end.$touch()"
                    :error-messages="years_clima_endErrors">                        
                </v-text-field>
            </v-flex>   
            <v-flex xs3 class="pl-3 pr-3">
                <v-text-field hide-no-data hide-selected dense color="#77b942" type="number" v-model="eventHourlyDistributions.years_actual_start" 
                    :value="eventHourlyDistributions.years_actual_start" label="First year of recent comp." title="First year of recent comparison period, from 1985 onwards." 
                    @input="$v.eventHourlyDistributions.years_actual_start.$touch()" @blur="$v.eventHourlyDistributions.years_actual_start.$touch()"
                    :error-messages="years_actual_startErrors">  
                </v-text-field>
            </v-flex>            
            <v-flex xs3 class="pl-3 pr-3">
                <v-text-field hide-no-data hide-selected dense color="#77b942" type="number" v-model="eventHourlyDistributions.years_actual_end" 
                    :value="eventHourlyDistributions.years_actual_end" label="Last year of recent comp." title="Last year of recent comparison period." 
                    @input="$v.eventHourlyDistributions.years_actual_end.$touch()" @blur="$v.eventHourlyDistributions.years_actual_end.$touch()"
                    :error-messages="years_actual_endErrors">
                </v-text-field>
            </v-flex>    
            <v-flex xs3 class="pl-3 pr-3">
                <v-text-field hide-no-data hide-selected dense color="#77b942" type="number" v-model="eventHourlyDistributions.threshold" 
                    :value="eventHourlyDistributions.threshold" label="Threshold" title="Threshold below/above which event happens." 
                    @input="$v.eventHourlyDistributions.threshold.$touch()" @blur="$v.eventHourlyDistributions.threshold.$touch()"
                    :error-messages="thresholdErrors">
                </v-text-field>
            </v-flex>    

            <v-flex xs3 class="pl-3 pr-3">
                <v-text-field hide-no-data hide-selected dense color="#77b942" type="number" v-model="eventHourlyDistributions.duration" 
                    :value="eventHourlyDistributions.duration" label="Duration" title="Duration threshold (hours) above which event happens." 
                    @input="$v.eventHourlyDistributions.duration.$touch()" @blur="$v.eventHourlyDistributions.duration.$touch()"
                    :error-messages="durationErrors">
                </v-text-field>
            </v-flex>

            <v-flex xs12 sm12 md12 lg12 class="text-xs-right pr-2" style="padding: 0px; margin-bottom: 5px;">
                <v-btn small round color="#27304c" :disabled="!eventHourlyDistributionsValid" :loading="isLoading" dark @click="runService()" title="Run service" >
                RUN
                </v-btn>
            </v-flex>

        </v-layout>
    </v-form> 
       
</template>

<script>
import { required, between, numeric } from 'vuelidate/lib/validators'
export default {
    name: "eventHourlyDistributions",    
    data: () => ({        
        isLoading: false,
        API_key: "1a98a8ef2598-EU-SG-testing", 
        eventHourlyDistributionsValid: false,                                 
        eventHourlyDistributions: {
            location_name: '',
            nameArr: ['Frost', 'Heat waves', 'Precipitation'],
            selectedName: "Frost",
            duration: "",
            month_start: 5,
            month_end: 10,
            event_aggrArr: [
                {
                    name: 'Number of events/season',
                    value: 'number%20of%20events%20%2F%20season'
                }, 
                {
                    name: 'Number of hours/season',
                    value: 'number%20of%20hours%20%2F%20season'
                },
                {
                    name: 'Number of days/season',
                    value: 'number%20of%20days%20%2F%20season'
                }
            ],
            selectedEvent:{
                name: 'Number of events/season',
                value: 'number%20of%20events%20%2F%20season'
            },             
            formatArr: ['png', 'json', 'highchartsHtml'],
            selectedFormat: 'png',
            years_clima_start:"",
            years_clima_end: "",
            years_actual_start: "",
            years_actual_end: "",      
            threshold: "",              
        }
    }),
    validations: {
       eventHourlyDistributions:{       
            month_start: {required, between: between(0, 12), numeric},
            month_end: {required, between: between(0, 12), numeric},                   
            threshold: {numeric},
            duration: {between: between(0, 24), numeric},
            years_clima_start: {between: between(1985, 2020), numeric},  
            years_clima_end: {between: between(1985, 2020), numeric},  
            years_actual_start: {between: between(1985, 2020), numeric},
            years_actual_end: {between: between(1985, 2020), numeric},  
        }
    },
    computed: {
        month_startErrors () {
            const errors = []
            if (!this.$v.eventHourlyDistributions.month_start.$dirty) return errors
            !this.$v.eventHourlyDistributions.month_start.required && errors.push('Required field.')
            !this.$v.eventHourlyDistributions.month_start.between && errors.push('Values from 0 to 12')
            !this.$v.eventHourlyDistributions.month_start.numeric && errors.push('Insert a number')
            return errors
        },
        month_endErrors () {
            const errors = []
            if (!this.$v.eventHourlyDistributions.month_end.$dirty) return errors
            !this.$v.eventHourlyDistributions.month_end.required && errors.push('Required field.')
            !this.$v.eventHourlyDistributions.month_end.between && errors.push('Values from 0 to 12')
            !this.$v.eventHourlyDistributions.month_end.numeric && errors.push('Insert a number')
            return errors
        },
        thresholdErrors () {
            const errors = []
            if (!this.$v.eventHourlyDistributions.month_start.$dirty) return errors
            !this.$v.eventHourlyDistributions.month_start.numeric && errors.push('Insert a number')
            return errors
        },        
        durationErrors () {
            const errors = []
            if (!this.$v.eventHourlyDistributions.month_start.$dirty) return errors
            !this.$v.eventHourlyDistributions.month_start.numeric && errors.push('Insert a number')
            !this.$v.eventHourlyDistributions.month_start.between && errors.push('Values from 0 to 24')
            return errors
        },           
        t_baseErrors () {
            const errors = []
            if (!this.$v.eventHourlyDistributions.t_base.$dirty) return errors
            !this.$v.eventHourlyDistributions.t_base.decimal && errors.push('Insert a number')
            return errors
        },
        t_maxErrors () {
            const errors = []
            if (!this.$v.eventHourlyDistributions.t_max.$dirty) return errors
            !this.$v.eventHourlyDistributions.t_max.decimal && errors.push('Insert a number')
            return errors
        },   
        years_clima_startErrors () {
            const errors = []
            if (!this.$v.eventHourlyDistributions.years_clima_start.$dirty) return errors
            !this.$v.eventHourlyDistributions.years_clima_start.between && errors.push('Values from 1985 to 2020')
            !this.$v.eventHourlyDistributions.years_clima_start.numeric && errors.push('Insert a number')
            return errors
        },     
        years_clima_endErrors () {
            const errors = []
            if (!this.$v.eventHourlyDistributions.years_clima_end.$dirty) return errors
            !this.$v.eventHourlyDistributions.years_clima_end.between && errors.push('Values from 1985 to 2020')
            !this.$v.eventHourlyDistributions.years_clima_end.numeric && errors.push('Insert a number')
            return errors
        },      
        years_actual_startErrors () {
            const errors = []
            if (!this.$v.eventHourlyDistributions.years_actual_start.$dirty) return errors
            !this.$v.eventHourlyDistributions.years_actual_start.between && errors.push('Values from 1985 to 2020')
            !this.$v.eventHourlyDistributions.years_actual_start.numeric && errors.push('Insert a number')
            return errors
        },    
        years_actual_endErrors () {
            const errors = []
            if (!this.$v.eventHourlyDistributions.years_actual_end.$dirty) return errors
            !this.$v.eventHourlyDistributions.years_actual_end.between && errors.push('Values from 1985 to 2020')
            !this.$v.eventHourlyDistributions.years_actual_end.numeric && errors.push('Insert a number')
            return errors
        },                  
    },
    methods: {   
        runService(){
            this.$v.$touch()
            var url = 'http://pyapi.meteoblue.com/';     
        
            url = url.concat('eventHourlyDistributions/', this.eventHourlyDistributions.selectedName, '/', this.$store.state.mapCoords.lat, 
                '/', this.$store.state.mapCoords.long, '/', this.eventHourlyDistributions.month_start, '/', 
                this.eventHourlyDistributions.month_end, '/', this.API_key, '?format=', this.eventHourlyDistributions.selectedFormat,
                '&event_aggr=', this.eventHourlyDistributions.selectedEvent.value);

            if(this.eventHourlyDistributions.years_clima_start){
                url = url.concat('&years_clima_start=', this.eventHourlyDistributions.years_clima_start)
            }
            if(this.eventHourlyDistributions.years_clima_end){
                url = url.concat('&years_clima_end=', this.eventHourlyDistributions.years_clima_end)
            }
            if(this.eventHourlyDistributions.years_actual_start){
                url = url.concat('&years_actual_start=', this.eventHourlyDistributions.years_actual_start)
            }
            if(this.eventHourlyDistributions.years_actual_end){
                url = url.concat('&years_actual_end=', this.eventHourlyDistributions.years_actual_end)
            }
            if(this.eventHourlyDistributions.location_name){
                url = url.concat('&location_name=', this.eventHourlyDistributions.location_name)
            }
            if(this.eventHourlyDistributions.threshold){
                url = url.concat('&threshold=', this.eventHourlyDistributions.threshold)
            }
            if(this.eventHourlyDistributions.duration){
                url = url.concat('&duration=', this.eventHourlyDistributions.duration)
            }   
            
            this.$eventBus.$emit('get-output', url, this.eventHourlyDistributions.selectedFormat);             
        }
    },

};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>