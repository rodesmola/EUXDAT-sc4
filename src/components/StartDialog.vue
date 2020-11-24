<template>

    <v-card-text>
        <v-flex xs12>
            <v-card flat class="pa-2">
                                    
                <div style="margin-bottom: 15px; margin-top: 15px">
                    <img style="width: 160px; position: absolute; opacity: 0.2; bottom: 5px; right: 5px;" src="../assets/logo2.png" alt="">
                    <span class="title" style="color: #1e2f4d;">Climate risk analysis</span>
                </div> 
                <p>
                    Today, climate change is affecting most areas in Europe. The magnitude of the changes is not yet fully known, 
                    however, the variability over the past 10 to 30 years is rather precisely understood. This knowledge 
                    can be used to mitigate climate change risks but must be made available in suitable ways.
                </p>
                <p>
                    A number of crop-specific and generic climate analysis services were developed in the EUXDAT project. 
                    The first three services available here allow you to assess climatic conditions: 
                </p>                
                <p>
                    <ol>
                    <li> <span style="font-style: italic;">"Crop climate risk analysis"</span>: the drought and frost risks for a particular crop at a particular 
                        location using different time periods as reference;</li>
                    <li><span style="font-style: italic;">"Climate pattern comparison"</span>: differences of climatic patterns for the major 
                        weather-related variables (temperature, precipitation, evapotranspiration, soil moisture) 
                        at a particular location for different time periods;</li>
                    <li><span style="font-style: italic;">"Generic climate risk assessment"</span>: climatic trends, frequency distributions and extremes for 
                        the major weather-related variables (temperature, precipitation, evapotranspiration, soil moisture) 
                        at a particular location for different time periods.</li>
                    </ol>
                </p>
                <p style="margin-bottom: 15px;">
                    These services support mid-term decisions such as which crops to plant at which locations, when to plant in the crop cycle, and so on.
                </p>
                
                <div style="margin-bottom: 15px; margin-top: 15px">                 
                    <span class="title" style="color: #1e2f4d;">Weather risk monitoring and prediction</span>
                </div> 
                <p>
                   A fourth service (<span style="font-style: italic;">“Crop weather risk monitoring and prediction”</span>) allows you to monitor the drought and frost risks 
                   for a particular crop at a particular location during its growth period. Data include the current season’s weather to date, 
                   a 7-day forecast, and a seasonal forecast. In addition, single or multiple reference years can be included for comparison. 
                   This service supports operational decisions such as when to plant, fertilize, irrigate, etc
                </p>
                <p>
                   All services can be run for any location in Europe and globally. Accuracy will vary depending on the accuracy of the available weather and climate data.
                </p>  
            </v-card>
        </v-flex>
    </v-card-text>

</template>

<script>
export default {
  name: "StartDialog",
  props: {}
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>