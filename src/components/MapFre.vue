<template>
  <div>
    <h1>Pourcentage d'objets perdus en fonction de la fréquendation</h1>
  <div class="body">
   <l-map :zoom="zoom" :center="center">
      <l-tile-layer :url="url"></l-tile-layer>
      <l-circle 
        v-for="(station, index) in data"
        :lat-lng="[station.lat, station.lon]"
        :radius="(parseInt(station.Perdu)/parseInt(station.voyageur))*100000"
        v-bind:key="index"
        :color="getColor(station.CODE_UIC)"
      >
        <l-tooltip>
          <p>
            <b>Gare</b> : {{station.LIBELLE}}
          </p>
          <p>
            <b>Nombre de voyageur</b> : {{station.voyageur}}
          </p>
          <p>
            <b>Nombre d'objet perdu</b> : {{station.Perdu}}
          </p>

          <p>
            <b>Ratio</b> : {{ parseInt(station.ratioPV)/parseInt(station.voyageur)}}
          </p>
        </l-tooltip>
      </l-circle> 
    </l-map>
  </div>
  </div>
</template>

<script>
import { LMap, LTileLayer, LCircle, LTooltip } from "vue2-leaflet";
export default {
  name: 'MapFre',
  props: {
    data: Array,
    gares: Array
  },
  components: {
    LMap,
    LTileLayer,
    LCircle,
    LTooltip
  }, 
  data() {
    return {
      zoom: 6,
      url: "http://{s}.tile.osm.org/{z}/{x}/{y}.png",
      center: [46.8,2.4350357]
    };
  },
  methods:{
    getColor(codeuic){
      let regions = {
        "REGION ALPES":"#E69F00",
        "REGION ALSACE":"#56B4E9",
        "REGION AUVERGNE":"#009E73",
        "REGION BOURGOGNE FRANCHE-COMTE":"#F0E442",
        "REGION BRETAGNE":"#0072B2",
        "REGION CENTRE":"#D55E00",
        "REGION CHAMPAGNE-ARDENNE":"#CC79A7",
        "REGION DE PARIS RIVE GAUCHE":"#E69F00",
        "REGION DE PARIS SAINT-LAZARE":"#56B4E9",
        "REGION DE PARIS-EST":"#009E73",
        "REGION DE PARIS-NORD":"#F0E442",
        "REGION DE PARIS-SUD-EST":"#0072B2",
        "REGION LANGUEDOC-ROUSSILLON":"#D55E00",
        "REGION LIMOUSIN":"#CC79A7",
        "REGION LORRAINE":"#E69F00",
        "REGION MIDI PYRENEES":"#56B4E9",
        "REGION NORD PAS DE CALAIS":"#009E73",
        "REGION NORMANDIE":"#F0E442",
        "REGION PAYS DE LA LOIRE":"#0072B2",
        "REGION PICARDIE":"#D55E00",
        "REGION POITOU-CHARENTES AQUITAINE":"#CC79A7",
        "REGION PROVENCE ALPES COTE D'AZUR":"#E69F00",
        "REGION RHONE-ALPES":"#56B4E9",
      }
      var r = this.gares.find((g) =>{
        if(g["Code UIC"]==codeuic){
          return g["Région SNCF"]
        }
      });
      if(r)
        return regions[r["Région SNCF"]]
      else  
        return "white"
    },
    ratioFloat(val){
      if(val.match(/E-0/g)){
        let l=  val.replace(',','.')
        return parseFloat(l.split("E-")[0])*(10**- parseInt(l.split("E-")[1]));
        
      }else{
        
        return parseFloat(val);
      }
    }

  
  }
}
</script>

<style scoped>
.body {
  width: 700px;
  height: 625px;
}
</style>

