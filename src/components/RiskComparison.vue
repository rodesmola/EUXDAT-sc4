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

        <v-flex xs12 pl-2 row v-if="selectedMbService.value == 'seasonAverages'">
            <v-layout row wrap>
                <!-- <p style="color: grey; font-size 8px; margin-bottom: 5px;" class="pl-3 pr-3">
                    <b>Season averages:</b>
                    The Season averages diagram shows the comparison, along a customisable growing season, 
                    of average curves of the selected weather variable relative to two different time periods 
                    (typically the last 10 years vs 10 years of the past). The distributions compare the 
                    Kernel Density Estimation for the two selected time periods.
                </p> -->

                <v-flex xs8 sm8 md6 lg8 xlg8 class="pl-3 pr-3">
                    <v-text-field hide-details hide-no-data hide-selected dense color="#77b942" type="text" v-model="seasonAverages.location_name" :value="seasonAverages.location_name"
                    label="Diagram title"></v-text-field>
                </v-flex>

                <v-flex sm9 xs6 md6 lg4 xlg4 class="pl-3 pr-3">
                    <v-combobox  hide-details hide-no-data hide-selected dense 
                    v-model="seasonAverages.selectedFormat"
                    :items="seasonAverages.formatArr"
                    item-text="name"
                    item-value="value"
                    label="Select diagram format"
                    color="green"
                    ></v-combobox>
                </v-flex>

                <v-flex sm9 xs6 md6 lg6 xlg6 class="pl-3 pr-3">
                    <v-combobox hide-details hide-no-data hide-selected dense 
                    v-model="seasonAverages.selectedType"
                    :items="seasonAverages.typeArr"
                    item-text="name"
                    item-value="value"
                    label="Select output diagram type"
                    color="green"
                    ></v-combobox>
                </v-flex>

                <v-flex sm9 xs6 md6 lg6 xlg6 class="pl-3 pr-3">
                    <v-combobox hide-details hide-no-data hide-selected dense 
                    v-model="seasonAverages.selectedVariableOfInt"
                    :items="seasonAverages.variableOfInterestArr"
                    item-text="name"
                    item-value="value"
                    label="Select variable of interest"
                    color="green"
                    ></v-combobox>
                </v-flex>

               <v-flex sm9 xs6 md6 lg6 xlg6 class="pl-3 pr-3">
                    <v-combobox hide-details hide-no-data hide-selected dense 
                    v-model="seasonAverages.selectedAggr"
                    :items="seasonAverages.aggrArr"
                    item-text="name"
                    item-value="value"
                    label="Select variable of interest"
                    color="green"
                    ></v-combobox>
                </v-flex>

                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field hide-details hide-no-data hide-selected dense color="#77b942" type="number" v-model="seasonAverages.month_start" :value="seasonAverages.month_start"
                    label="Initial month" title="Initial month of growing season" :rules="inputNumRules"></v-text-field>
                </v-flex>
                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field hide-details hide-no-data hide-selected dense color="#77b942" type="number" v-model="seasonAverages.month_end" :value="seasonAverages.month_end"
                    label="Final month" title="Final month of growing season" :rules="inputNumRules"></v-text-field>
                </v-flex>

                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field hide-details hide-no-data hide-selected dense color="#77b942" type="number" v-model="seasonAverages.t_base" :value="seasonAverages.t_base"
                    label="Base temp" :rules="inputNumRules"></v-text-field>
                </v-flex>
                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field hide-details hide-no-data hide-selected dense color="#77b942" type="number" v-model="seasonAverages.t_max" :value="seasonAverages.t_max"
                    label="Max temp" :rules="inputNumRules"></v-text-field>
                </v-flex>

                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field hide-details hide-no-data hide-selected dense color="#77b942" type="number" v-model="seasonAverages.years_clima_start" :value="seasonAverages.years_clima_start"
                    label="First year comp." title="First year of past comparison period, from 1985 onwards." :rules="inputNumRules"></v-text-field>
                </v-flex>  
                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field hide-details hide-no-data hide-selected dense color="#77b942" type="number" v-model="seasonAverages.years_clima_end" :value="seasonAverages.years_clima_end"
                    label="Last year comp." title="Last year of past comparison period." :rules="inputNumRules"></v-text-field>
                </v-flex>   
                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field hide-details hide-no-data hide-selected dense color="#77b942" type="number" v-model="seasonAverages.years_actual_start" :value="seasonAverages.years_actual_start"
                    label="First year of recent comp." title="First year of recent comparison period, from 1985 onwards." :rules="inputNumRules"></v-text-field>
                </v-flex>            
                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field hide-details hide-no-data hide-selected dense color="#77b942" type="number" v-model="seasonAverages.years_actual_end" :value="seasonAverages.years_actual_end"
                    label="Last year of recent comp." title="Last year of recent comparison period." :rules="inputNumRules"></v-text-field>
                </v-flex>    
            </v-layout>
        </v-flex>

        <v-flex xs12 pl-2 row v-if="selectedMbService.value == 'yearlyAverages'">
            <v-layout row wrap>

                <v-flex xs8 sm8 md6 lg8 xlg8 class="pl-3 pr-3">
                    <v-text-field hide-details hide-no-data hide-selected dense color="#77b942" type="text" v-model="yearlyAverages.location_name" :value="yearlyAverages.location_name"
                    label="Diagram title"></v-text-field>
                </v-flex>

                <v-flex sm9 xs6 md6 lg4 xlg4 class="pl-3 pr-3">
                    <v-combobox hide-details hide-no-data hide-selected dense
                    v-model="yearlyAverages.selectedFormat"
                    :items="yearlyAverages.formatArr"
                    item-text="name"
                    item-value="value"
                    label="Select diagram format"
                    color="green"
                    ></v-combobox>
                </v-flex>

                <v-flex sm9 xs6 md6 lg6 xlg6 class="pl-3 pr-3">
                    <v-combobox hide-details hide-no-data hide-selected dense
                    v-model="yearlyAverages.selectedType"
                    :items="yearlyAverages.typeArr"
                    item-text="name"
                    item-value="value"
                    label="Select output diagram type"
                    color="green"
                    ></v-combobox>
                </v-flex>
                <v-flex sm9 xs6 md6 lg6 xlg6 class="pl-3 pr-3">
                    <v-combobox hide-details hide-no-data hide-selected dense
                    v-model="yearlyAverages.selectedName"
                    :items="yearlyAverages.nameArr"
                    item-text="name"
                    item-value="value"
                    label="Variable of interest"
                    color="green"
                    ></v-combobox>
                </v-flex>

                <v-flex sm9 xs6 md6 lg6 xlg6 class="pl-3 pr-3" v-if="yearlyAverages.selectedName === 'Temperature'">
                    <v-combobox hide-details hide-no-data hide-selected dense
                    v-model="yearlyAverages.selectedAggr"
                    :items="yearlyAverages.aggregationArr"
                    item-text="name"
                    item-value="value"
                    label="Variable of interest"
                    color="green"
                    ></v-combobox>
                </v-flex>                
                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field hide-details hide-no-data hide-selected dense color="#77b942" type="number" v-model="yearlyAverages.years_clima_start" :value="yearlyAverages.years_clima_start"
                    label="First year comp." title="First year of past comparison period, from 1985 onwards." :rules="inputNumRules"></v-text-field>
                </v-flex>  
                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field hide-details hide-no-data hide-selected dense color="#77b942" type="number" v-model="yearlyAverages.years_clima_end" :value="yearlyAverages.years_clima_end"
                    label="Last year comp." title="Last year of past comparison period." :rules="inputNumRules"></v-text-field>
                </v-flex>   
                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field hide-details hide-no-data hide-selected dense color="#77b942" type="number" v-model="yearlyAverages.years_actual_start" :value="yearlyAverages.years_actual_start"
                    label="First year of recent comp." title="First year of recent comparison period, from 1985 onwards." :rules="inputNumRules"></v-text-field>
                </v-flex>            
                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field hide-details hide-no-data hide-selected dense color="#77b942" type="number" v-model="yearlyAverages.years_actual_end" :value="yearlyAverages.years_actual_end"
                    label="Last year of recent comp." title="Last year of recent comparison period." :rules="inputNumRules"></v-text-field>
                </v-flex>    
            </v-layout>
        </v-flex>

        <v-flex xs12 pl-2 row v-if="selectedMbService.value == 'eventDailyDistributions'">
            <v-layout row wrap>

                <v-flex xs8 sm8 md6 lg8 xlg8 class="pl-3 pr-3">
                    <v-text-field hide-details hide-no-data hide-selected dense color="#77b942" type="text" v-model="eventDailyDistributions.location_name" :value="eventDailyDistributions.location_name"
                    label="Diagram title"></v-text-field>
                </v-flex>

                <v-flex sm9 xs6 md6 lg4 xlg4 class="pl-3 pr-3">
                    <v-combobox hide-details hide-no-data hide-selected dense
                    v-model="eventDailyDistributions.selectedFormat"
                    :items="eventDailyDistributions.formatArr"
                    item-text="name"
                    item-value="value"
                    label="Select diagram format"
                    color="green"
                    ></v-combobox>
                </v-flex>
                <v-flex sm9 xs6 md6 lg6 xlg6 class="pl-3 pr-3">
                    <v-combobox hide-details hide-no-data hide-selected dense
                    v-model="eventDailyDistributions.selectedEvent"
                    :items="eventDailyDistributions.event_aggrArr"
                    item-text="name"
                    item-value="value"
                    label="Aggregation of event"
                    color="green"
                    ></v-combobox>
                </v-flex>

                <v-flex sm9 xs6 md6 lg6 xlg6 class="pl-3 pr-3">
                    <v-combobox hide-details hide-no-data hide-selected dense
                    v-model="eventDailyDistributions.selectedName"
                    :items="eventDailyDistributions.nameArr"
                    item-text="name"
                    item-value="value"
                    label="Variable of interest"
                    color="green"
                    ></v-combobox>
                </v-flex>

                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field hide-details hide-no-data hide-selected dense color="#77b942" type="number" v-model="eventDailyDistributions.month_start" :value="seasonAverages.month_start"
                    label="Initial month" title="Initial month of growing season" :rules="inputNumRules"></v-text-field>
                </v-flex>
                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field hide-details hide-no-data hide-selected dense color="#77b942" type="number" v-model="eventDailyDistributions.month_end" :value="seasonAverages.month_end"
                    label="Final month" title="Final month of growing season" :rules="inputNumRules"></v-text-field>
                </v-flex>

                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field hide-details hide-no-data hide-selected dense color="#77b942" type="number" v-model="eventDailyDistributions.years_clima_start" :value="eventDailyDistributions.years_clima_start"
                    label="First year comp." title="First year of past comparison period, from 1985 onwards." :rules="inputNumRules"></v-text-field>
                </v-flex>  
                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field hide-details hide-no-data hide-selected dense color="#77b942" type="number" v-model="eventDailyDistributions.years_clima_end" :value="eventDailyDistributions.years_clima_end"
                    label="Last year comp." title="Last year of past comparison period." :rules="inputNumRules"></v-text-field>
                </v-flex>   
                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field hide-details hide-no-data hide-selected dense color="#77b942" type="number" v-model="eventDailyDistributions.years_actual_start" :value="eventDailyDistributions.years_actual_start"
                    label="First year of recent comp." title="First year of recent comparison period, from 1985 onwards." :rules="inputNumRules"></v-text-field>
                </v-flex>            
                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field hide-details hide-no-data hide-selected dense color="#77b942" type="number" v-model="eventDailyDistributions.years_actual_end" :value="eventDailyDistributions.years_actual_end"
                    label="Last year of recent comp." title="Last year of recent comparison period." :rules="inputNumRules"></v-text-field>
                </v-flex>    

                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field hide-details hide-no-data hide-selected dense color="#77b942" type="number" v-model="eventDailyDistributions.threshold" :value="eventDailyDistributions.threshold"
                    label="Threshold" title="Threshold below/above which event happens." :rules="inputNumRules"></v-text-field>
                </v-flex>    
    
                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field hide-details hide-no-data hide-selected dense color="#77b942" type="number" v-model="eventDailyDistributions.duration" :value="eventDailyDistributions.duration"
                    label="Duration" title="Duration threshold (hours) above which event happens." :rules="inputNumRules"></v-text-field>
                </v-flex>

                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field hide-details hide-no-data hide-selected dense color="#77b942" type="number" v-model="eventDailyDistributions.var_maximum" :value="eventDailyDistributions.var_maximum"
                    label="Maximum possible variable value" title="Maximum possible variable value (when meaningful)." :rules="inputNumRules"></v-text-field>
                </v-flex>

            </v-layout>
        </v-flex>

        <v-flex xs12 pl-2 row v-if="selectedMbService.value == 'eventHourlyDistributions'">
            <v-layout row wrap>

                <v-flex xs8 sm8 md6 lg8 xlg8 class="pl-3 pr-3">
                    <v-text-field hide-details hide-no-data hide-selected dense color="#77b942" type="text" v-model="eventHourlyDistributions.location_name" :value="eventHourlyDistributions.location_name"
                    label="Diagram title"></v-text-field>
                </v-flex>
                <v-flex sm9 xs6 md6 lg4 xlg4 class="pl-3 pr-3">
                    <v-combobox hide-details hide-no-data hide-selected dense
                    v-model="eventHourlyDistributions.selectedFormat"
                    :items="eventHourlyDistributions.formatArr"
                    item-text="name"
                    item-value="value"
                    label="Select diagram format"
                    color="green"
                    ></v-combobox>
                </v-flex>
                <v-flex sm9 xs6 md6 lg6 xlg6 class="pl-3 pr-3">
                    <v-combobox hide-details hide-no-data hide-selected dense
                    v-model="eventHourlyDistributions.selectedName"
                    :items="eventHourlyDistributions.nameArr"
                    item-text="name"
                    item-value="value"
                    label="Variable of interest"
                    color="green"
                    ></v-combobox>
                </v-flex>
                <v-flex sm9 xs6 md6 lg6 xlg6 class="pl-3 pr-3">
                    <v-combobox hide-details hide-no-data hide-selected dense
                    v-model="eventHourlyDistributions.selectedEvent"
                    :items="eventHourlyDistributions.event_aggrArr"
                    item-text="name"
                    item-value="value"
                    label="Aggregation of event"
                    color="green"
                    ></v-combobox>
                </v-flex>

                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field hide-details hide-no-data hide-selected dense color="#77b942" type="number" v-model="eventHourlyDistributions.month_start" :value="eventHourlyDistributions.month_start"
                    label="Initial month" title="Initial month of growing season" :rules="inputNumRules"></v-text-field>
                </v-flex>
                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field hide-details hide-no-data hide-selected dense color="#77b942" type="number" v-model="eventHourlyDistributions.month_end" :value="eventHourlyDistributions.month_end"
                    label="Final month" title="Final month of growing season" :rules="inputNumRules"></v-text-field>
                </v-flex>
                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field hide-details hide-no-data hide-selected dense color="#77b942" type="number" v-model="eventHourlyDistributions.years_clima_start" :value="eventHourlyDistributions.years_clima_start"
                    label="First year comp." title="First year of past comparison period, from 1985 onwards." :rules="inputNumRules"></v-text-field>
                </v-flex>  
                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field hide-details hide-no-data hide-selected dense color="#77b942" type="number" v-model="eventHourlyDistributions.years_clima_end" :value="eventHourlyDistributions.years_clima_end"
                    label="Last year comp." title="Last year of past comparison period." :rules="inputNumRules"></v-text-field>
                </v-flex>   
                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field hide-details hide-no-data hide-selected dense color="#77b942" type="number" v-model="eventHourlyDistributions.years_actual_start" :value="eventHourlyDistributions.years_actual_start"
                    label="First year of recent comp." title="First year of recent comparison period, from 1985 onwards." :rules="inputNumRules"></v-text-field>
                </v-flex>            
                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field hide-details hide-no-data hide-selected dense color="#77b942" type="number" v-model="eventHourlyDistributions.years_actual_end" :value="eventHourlyDistributions.years_actual_end"
                    label="Last year of recent comp." title="Last year of recent comparison period." :rules="inputNumRules"></v-text-field>
                </v-flex>    
                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field hide-details hide-no-data hide-selected dense color="#77b942" type="number" v-model="eventHourlyDistributions.threshold" :value="eventHourlyDistributions.threshold"
                    label="Threshold" title="Threshold below/above which event happens." :rules="inputNumRules"></v-text-field>
                </v-flex>    
    
                <v-flex xs3 class="pl-3 pr-3">
                    <v-text-field hide-details hide-no-data hide-selected dense color="#77b942" type="number" v-model="eventHourlyDistributions.duration" :value="eventHourlyDistributions.duration"
                    label="Duration" title="Duration threshold (hours) above which event happens." :rules="inputNumRules"></v-text-field>
                </v-flex>

            </v-layout>
        </v-flex>

        <v-flex xs12 sm12 md12 lg12 class="text-xs-right mt-1" style="padding: 0px; margin-bottom: 5px;">
            <v-btn small round color="#27304c" :disabled="!selectedMbService" :loading="isLoading" dark @click="runService()" title="Run service" >
            RUN
            </v-btn>
        </v-flex>
    </div>


    </div>

</template>

<script>

export default {
    name: "RiskComparison",    
    data: () => ({
        inputNumRules: [            
            v => (v && /^\d+(\.\d{1,20})?$/.test(v)) || ''
        ], 
        isLoading: false,
        API_key: "1a98a8ef2598-EU-SG-testing",
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
        selectedMbService:"",                                    
        seasonAverages:{
            variableOfInterestArr: ['GDD', 'Precipitation', 'Radiation', 'Evapotranspiration'],
            selectedVariableOfInt:'GDD',
            month_start: 5,
            month_end: 10,
            selectedFormat: 'png',
            formatArr: ['png', 'json', 'highchartsHtml'],
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
        },
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
            years_clima_start:1985,
            years_clima_end: 1995,
            years_actual_start: 2010,
            years_actual_end: 2020
        },
        eventDailyDistributions: {
            location_name: '',
            nameArr: ['Frost', 'Icing', 'Heat waves', 'Precipitation', 'Drought', 'Tropical nights'],
            selectedName: "Precipitation",
            selectedFormat: 'png',
            formatArr: ['png', 'json', 'highchartsHtml'],
            month_start: 5,
            month_end: 10,
            years_clima_start:1985,
            years_clima_end: 1995,
            years_actual_start: 2010,
            years_actual_end: 2020,
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
            years_clima_start:1985,
            years_clima_end: 1995,
            years_actual_start: 2010,
            years_actual_end: 2020,      
            threshold: "",              
        }

    }),
    methods: {  
        runService(){

            var url = 'http://pyapi.meteoblue.com/';

            if(this.selectedMbService.value === 'seasonAverages'){

                url = url.concat('seasonAverages/', this.seasonAverages.selectedVariableOfInt, '/', this.$store.state.mapCoords.lat, '/', this.$store.state.mapCoords.long, 
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
            }
            if(this.selectedMbService.value === 'yearlyAverages'){

                if(this.yearlyAverages.selectedName === 'Temperature'){
                    url = url.concat('yearlyAveragesTemp/', this.yearlyAverages.selectedName, '/', this.$store.state.mapCoords.lat, '/', this.$store.state.mapCoords.long, 
                    '/', this.yearlyAverages.selectedAggr, '/')
                }else{
                    url = url.concat('yearlyAveragesOther/', this.yearlyAverages.selectedName, '/', this.$store.state.mapCoords.lat, '/', this.$store.state.mapCoords.long, '/')        
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
            }

            if(this.selectedMbService.value === 'eventDailyDistributions'){

                url = url.concat('eventDailyDistributions/', this.eventDailyDistributions.selectedName, '/', this.$store.state.mapCoords.lat, '/', this.$store.state.mapCoords.long, 
                    '/', this.eventDailyDistributions.month_start, '/', this.eventDailyDistributions.month_end, '/', this.API_key, '?format=', this.eventDailyDistributions.selectedFormat,
                    '&event_aggr=', this.eventDailyDistributions.selectedEvent.value);

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
            }
            if(this.selectedMbService.value === 'eventHourlyDistributions'){

                url = url.concat('eventHourlyDistributions/', this.eventHourlyDistributions.selectedName, '/', this.$store.state.mapCoords.lat, '/', this.$store.state.mapCoords.long, 
                    '/', this.eventHourlyDistributions.month_start, '/', this.eventHourlyDistributions.month_end, '/', this.API_key, '?format=', this.eventDailyDistributions.selectedFormat,
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
            }

            console.log(url)

            this.$http.get(url).then(response => {                                                    
            }, response => {
                this.isLoading = false;                    
                this.$eventBus.$emit('show-alert', "error", response.statusText); 
            });

        }
    },
    created(){
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


