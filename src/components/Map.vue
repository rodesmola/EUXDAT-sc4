<template style="overflow: hidden">
<!-- eslint-disable vue/no-use-v-if-with-v-for,vue/no-confusing-v-for-v-if -->

  <v-container fluid grid-list-md fill-height style="padding: 0px;">
    <v-layout row wrap style="padding: 0px;">

      <v-flex fill-height ma-0 pa-0>
        <!------------ Start dialog ------------>
        <v-dialog v-model="startDialog" max-width="800">
          <v-card>
            <v-card-title class="headline">
                <img style="width: 155px;" src="../assets/logo_titulo.png" alt="">
            </v-card-title>

            <v-divider></v-divider>
              <StartDialog/>
            <v-divider></v-divider>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="green darken-1" dark small @click="startDialog = false">
                Got it!
              </v-btn>
             </v-card-actions>
           </v-card>
        </v-dialog>
        <!------------ Start dialog end ------------>

        <v-toolbar color="white" xs12 tabs  height="58px" style="position: absolute; z-index: 10; border-bottom: 4px solid #098837 !important;">
          <v-toolbar-title>
            <a href="https://test.euxdat.eu/">
              <img style="width: 140px;" src="http://www.euxdat.eu/wp-content/uploads/2017/12/logo_1-1.png" alt="">
            </a>
          </v-toolbar-title>
          <v-spacer></v-spacer>
          <span style="color:#27304c">
            <v-icon>person</v-icon> <span v-if="!$vuetify.breakpoint.smAndDown">{{user.name}}</span>
          </span>
          <v-btn icon title="Exit app" @click="logout()">
            <v-icon color="#27304c">exit_to_app</v-icon>
          </v-btn>
        </v-toolbar>

        <v-flex id="map" style="max-height: 100vh; height: 100vh; padding: 0px; margin: 0px;">
          <div id="mapsight"></div>
          <!------------ Service form start ------------>
          <div class="flex xs12 sm5 md4 lg4 xl3" style="position: absolute; z-index: 10; top:80px; left: 10px; background-color: #27304c;">
            <v-toolbar class="green" tabs  height="42px">
              <v-toolbar-title>
                <img style="width: 30px;" src="../assets/logo_1-1.png" alt="">
                <span style="font-size: 18px; margin-left: 5px;">Climate risk analysis and mitigation</span>
              </v-toolbar-title>

              <v-spacer></v-spacer>

              <v-btn icon @click="startDialog = true" title="More info">
                <v-icon color="#27304c">info</v-icon>
              </v-btn>

            </v-toolbar>

            <div style="background-color: white; padding-left: 10px; padding-right: 10px; overflow:auto; max-height: 80vh;">

              <div ref="form" style="margin-top: 10px; margin-bottom: 5px; text-size: 10px;">
                <v-layout row wrap style="text-align: left; padding-top: 8px; padding-bottom: 8px;">

                  <v-expansion-panel>
                    <v-expansion-panel-content expand-icon="mdi-menu-down" v-for="(item,i) in panels" :key="i" >
                     
                      <template v-slot:header >                        
                        <div class="exp-tittle" @click="updateComponent(i)">{{item.name}}</div>                      
                      </template>              
                      <div v-if="i === 0" :key="componetRAkey">                        
                        <RiskAnalysis/>
                      </div>     
                      <div v-if="i === 1" :key="componetRCkey">
                        <RiskComparison/>
                      </div>                                                                     
                      <div v-if="i === 2" :key="componetCPkey">
                        <ClimaticPatterns/>
                      </div>
                      <div v-if="i === 3" :key="componetCWkey">
                        <CropWeather/>
                      </div>                      
                    </v-expansion-panel-content>
                  </v-expansion-panel>
                  
                </v-layout>
              </div>

            </div>
          </div>
          <!------------/Service form------------>

          <!------------ Zoom controls and layer manager start ------------>
          <div class="flex xs12 sm4 md3 lg2" style="position: absolute; z-index: 10; top:80px; right: 10px; ">
            <v-layout row wrap>
              <v-flex xs12 pl-2 row class="text-xs-right">

                <v-btn-toggle multiple>
                  <v-btn flat class="v-btn--active" title="Zoom in" @click="zoomMap(map, 'in')" style="background-color: #47a34b; color: white;">
                    <v-icon>add</v-icon>
                  </v-btn>
                  <v-btn flat class="v-btn--active" title="Zoom out" @click="zoomMap(map, 'out')" style="background-color: #47a34b; color: white;">
                    <v-icon>remove</v-icon>
                  </v-btn>
                </v-btn-toggle>

                <div class="" style="background-color: white; padding-left: 10px; padding-right: 10px;">
                  <v-radio-group hide-details hide-no-data hide-selected  v-model="selectedBaseLayer" :mandatory="false" v-on:change="setLayerVisibility(map)" style="padding-top: 14px;">
                    <v-radio color="#47a34b" label="Open Street Map" value="osm"></v-radio>
                    <v-radio color="#47a34b" label="Aerial" value="aerial" ></v-radio>
                  </v-radio-group>
                </div>

                <v-flex xs12 style="background-color: white;">
                  <v-divider class="pl-2 pr-2"></v-divider>
                </v-flex> 

                <v-flex xs12  row style="background-color: white;">

                  <v-form ref="mapCoordsForm" v-model="mapCoordsValid">                
                    <v-layout row wrap>
          
                      <v-flex xs6 class="pl-2 pr-2">
                        <v-btn dark small block color="#27304c" @click="centerMap(true)" title="Center map on initial position.">
                          <v-icon dark>home</v-icon>
                        </v-btn>
                      </v-flex>
                      <v-flex xs6 class="pl-2 pr-2">
                        <v-btn dark small block style="background-color: #47a34b; color: white;" @click="centerMap(false)" title="Center map on coordinates above.">
                          <v-icon dark>my_location</v-icon>
                        </v-btn>
                      </v-flex>  

                      <v-flex xs6 class="pl-2 pr-2">
                        <v-text-field hide-no-data hide-selected dense color="#77b942" type="text" v-model="mapCoords.lat" :value="mapCoords.lat"
                          label="Latitude" @input="$v.mapCoords.lat.$touch()" @blur="$v.mapCoords.lat.$touch()"
                          :error-messages="latErrors">
                        </v-text-field>
                      </v-flex>
                      <v-flex xs6 class="pl-2 pr-2">
                        <v-text-field hide-no-data hide-selected dense color="#77b942" type="text" v-model="mapCoords.long" :value="mapCoords.long"
                          label="Longitude" @input="$v.mapCoords.long.$touch()" @blur="$v.mapCoords.long.$touch()"
                          :error-messages="longErrors">
                        </v-text-field>
                      </v-flex>

                    </v-layout>
                  </v-form>

                </v-flex>     

              </v-flex>
              
            </v-layout>
          </div>
          <!------------ Zoom controls and layer manager end ------------>

          <!------------ Alert start ------------>
          <div class="flex xs3" style="left: 10px; bottom: 10px; position: absolute; z-index: 10;" >
            <v-alert :value="isAlert" :type="alertType" dismissible transition="scale-transition">
               {{alertMsg}}
            </v-alert>
          </div>
          <!------------ Alert end ------------>

        </v-flex>
      </v-flex>

    </v-layout>
  </v-container>

</template>

<script>

import Map from 'ol/Map.js';
import View from 'ol/View.js';
import {Tile as TileLayer, Vector as VectorLayer} from 'ol/layer.js';
import {Vector as VectorSource} from 'ol/source.js'
import {Fill, Stroke, Style} from 'ol/style.js';
import OSM from 'ol/source/OSM';
import BingMaps from 'ol/source/BingMaps.js';
import {required, decimal, between} from 'vuelidate/lib/validators'
import StartDialog from '@/components/StartDialog.vue'
import ClimaticPatterns from '@/components/ClimaticPatterns.vue'
import RiskAnalysis from '@/components/RiskAnalysis.vue'
import RiskComparison from '@/components/RiskComparison.vue'
import CropWeather from '@/components/CropWeather.vue'

export default {
  name: 'Map',
  components: {
    StartDialog,
    ClimaticPatterns,
    RiskAnalysis,
    RiskComparison,
    CropWeather
  },
  data: () => ({
    startDialog: true,
    mapCoordsValid: false,
    selectedBaseLayer: 'aerial',
    panels: [      
      {"name": "Crop climate risk analysis"},    
      {"name": "Climate pattern comparison"},  
      {"name": "Generic climate risk assessment"},
      {"name": "Crop weather risk monitoring and prediction"},
    ],   
    isAlert: false,
    alertMsg: "",
    alertType: "error",
    componetRAkey: 0,
    componetCPkey: 0,
    componetRCkey: 0,
    componetCWkey: 0,
    mapCoords: {
      lat: 0,
      long: 0,
    },

  }),
  methods: {
    /**
    * Re-render the entire component, takes index of the accordion
    *
    * @param {number} i
    * @public
    */
    updateComponent(i){
      if(i === 0){
        this.componetRAkey ++
        //this.$eventBus.$emit('updateComponetRA', this.componetRAkey);
      }else if (i === 1){
        this.componetRCkey ++       
        //this.$eventBus.$emit('updateComponetMZ', this.componetMZkey);
      }else if (i === 2) {
        this.componetCPkey ++
      } else {
        this.componetCWkey ++
      }
    },
    /**
    * Initialize Map, base layer, styles and select interaction
    *
    * @public
    */
    initMap() {
      
      var defaultStyle = new Style({
        stroke: new Stroke({
          color: '#3994bd',
          width: 2
        }),
        fill: new Fill({
          color: 'rgba(0, 0, 0, 0)'
        })
      });

      //Aerial layer
      var aerialLayer =  new TileLayer({
        visible: true,
        preload: Infinity,
        name: 'aerial',
        source: new BingMaps({
          key: 'AgbuLm2QSg5-rZyOrydftTOzC7z5pYY82q4By3PgFoKhPxNSrrcf1JFs1yoJXJIp',
          imagerySet: 'Aerial',
          maxZoom: 19
        })
      });

      //OSM layer
      var osm = new TileLayer({
        name: 'osm',
        source: new OSM()
      })      
      osm.setVisible(false);

      var drawLayer = new VectorLayer({
        source: new VectorSource(),
        name: 'userPolygonsLayer',
        style: defaultStyle
      });
      drawLayer.setZIndex(99)

      var myMap = new Map({
        target: 'map',
        layers: [
          aerialLayer,
          drawLayer,
          osm
        ],
        view: new View({
          projection: 'EPSG:4326', 
          center: [12.141, 48.512],          
          zoom: 11,
          //minZoom: 8,
        })
      });
      
      this.map = myMap;
      this.$store.state.map = myMap;

      var self = this;
      myMap.on('moveend', function () {  
        var center = myMap.getView().getCenter(); 
        self.mapCoords.lat = center[1].toString().substring(0, 6)
        self.mapCoords.long = center[0].toString().substring(0, 6)   
        self.$store.state.mapCoords = self.mapCoords;
      });

    },//initMap
    centerMap(isHome){

      if(!this.$v.$invalid){
        if(isHome){
          this.mapCoords.long = 12.141
          this.mapCoords.lat = 48.512
        }
        this.map.getView().animate({
          center: [this.mapCoords.long, this.mapCoords.lat],
          duration: 1000,
        });
      }else{
        this.showAlert("error", "Please insert correct coordinates");
      }
    },
    /**
    * zoom map controls
    *
    * @param {object} map
    * @param {string} op
    * @public
    */
    zoomMap(map, op){
      var zoom = map.getView().getZoom();
      if(op === "in"){
        map.getView().setZoom(zoom + 1)
      }else{
        map.getView().setZoom(zoom - 1)
      }
    },//zoomInMap
    /**
    * Get the map layer by name and return it as a OL layer object
    *        
    * @param {string} name
    * @return {object}
    * @public
    */
    getLayerFromMapByName(name){
        var layer;
        this.$store.state.map.getLayers().forEach(function(lyr) {
            if(lyr.get('name') === name){
                layer = lyr;
            }
        });
        return layer;
    },//getLayerFromMapByName    
    /**
    * Controls the base layers visibility
    *
    * @public
    */
    setLayerVisibility(){
      var self = this;
      if(this.selectedBaseLayer === 'osm'){
        self.getLayerFromMapByName('aerial').setVisible(false);
        self.getLayerFromMapByName('osm').setVisible(true);
      }else{
        self.getLayerFromMapByName('aerial').setVisible(true);
        self.getLayerFromMapByName('osm').setVisible(false);
      }
    },//setLayerVisibility
    /**
    * Create an alert with custom message
    *
    * @param {string} type
    * @param {string} msg
    * @public
    */
    showAlert(type, msg){
      var self = this;
      this.isAlert = true;
      this.alertMsg = msg;
      this.alertType = type;
      setTimeout(function(){ self.isAlert = false; }, 8000);
    },//showAlert

    /**
    * Logout the application using the Keycloak JavaScript adapter
    *
    * @public
    */
    logout: function(){
      this.$keycloak.logoutFn();
    },//logout
   },
  mounted: function(){
    this.initMap();
  },
  created(){
    
    this.user = JSON.parse(window.atob(this.$keycloak.token.split('.')[1].replace(/-/g, '+').replace(/_/g, '/')));
    this.$store.state.user = this.user;

    this.$eventBus.$on('show-alert', (type, msg)  => {
      this.showAlert(type, msg);
    });

  },
  validations: {
    mapCoords: {
      lat: {required, decimal, between: between(-90, 90)},
      long: {required, decimal, between: between(-180, 180)},
    }
  },
  computed: {
    latErrors () {
      const errors = []
      if (!this.$v.mapCoords.lat.$dirty) return errors
      !this.$v.mapCoords.lat.required && errors.push('Required field.')
      !this.$v.mapCoords.lat.between && errors.push('Values from -90 to 90')
      !this.$v.mapCoords.lat.decimal && errors.push('Insert a number')
      return errors
    },
    longErrors () {
      const errors = []
      if (!this.$v.mapCoords.long.$dirty) return errors
      !this.$v.mapCoords.long.required && errors.push('Required field.')
      !this.$v.mapCoords.long.between && errors.push('Values from -180 to 180')
      !this.$v.mapCoords.long.decimal && errors.push('Insert a number')
      return errors
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.green {
  background: linear-gradient(60deg,#66bb6a,#43a047) !important;
	-webkit-box-shadow: 0 12px 20px -10px rgba(76,175,80,.28),0 4px 20px 0 rgba(0,0,0,.12),0 7px 8px -5px rgba(76,175,80,.2) !important;
	box-shadow: 0 12px 20px -10px rgba(76,175,80,.28),0 4px 20px 0 rgba(0,0,0,.12),0 7px 8px -5px rgba(76,175,80,.2) !important;
}

.exp-tittle {
  color: #37aa48; 
  font-size: 20px !important;	
  font-family: Roboto,sans-serif !important; 
  font-weight: 500; line-height: 1 !important; 	
  letter-spacing: .02em !important;
}

#mapsight {
	position: absolute;
	top: 50%;
	left: 50%;
	width: 51px;
	height: 51px;
	margin: -26px;
	pointer-events: none;
	z-index: 100;
	background: url("../assets/epsg-target-large.png") 0 0 no-repeat;
}

</style>