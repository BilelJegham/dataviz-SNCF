<template>
  <div id="app">
    <div v-if="!loading">
      <MapPerdu  :data="filteredItems" :gares="gares"/>
      <MapPerduRetrouve :data="filteredItems" :gares="gares"/>
      <MapFre :data="filteredItems" :gares="gares"/>
    </div>
    <div v-else class="loading">
      <p>
        Chargement des données, veuillez patienter ...<br/>
        {{this.data.length +this.gares.length}} / 7552 <br/>
        🚂🚉🚆🚞
      </p>
    </div>
    <div class="footer">
      <p>Réalisé par Bilel Jegham et Corentin Goinard | Source : <a href="https://data.sncf.com/">https://data.sncf.com/</a></p>
    </div>
  </div>
</template>

<script>

import * as d3 from "d3"
import MapPerdu from "./components/MapPerdu"
import MapPerduRetrouve from "./components/MapPerduRetrouve"
import MapFre from "./components/MapFre"

export default {
  name: 'app',
  components: {
    MapPerdu,
    MapPerduRetrouve,
    MapFre
  },
  data: function() {
    return {
      data: [],
      gares: []
    };
  },  
  created() {
   var t = this
    d3.csv(
        "./table.csv"
    ).then(function(data) {
      t.data= data
    })
        d3.csv(
        "./referentiel-gares-voyageurs.csv"
        ).then(function(data) {
          t.gares= data
        });
 

  },
  computed: {
    loading(){
      return (this.gares.length == 0 || this.data.length == 0)

    },
      filteredItems() {
        return this.data.filter((thing, index, self) =>
          index === self.findIndex((t) => (
            t.CODE_UIC === thing.CODE_UIC 
          )))
      },
      count(){
        return this.data.length +this.gares.length
      }
  
  }
}
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  height: 100%;
}
html{ 
  height: 100%;
}
html,
body, h3 {
  margin: 0;
  padding: 0;
  height: 100%;
}
.loading{
  height: 90%;
  display: flex;
}
.loading p{
  margin: auto;
  font-size: 2em;
  text-align: center
}
.footer{
  background: #222;
  color: white;
  padding: 0.5%;
  margin-top: 2%;

}
.footer a{
  color : white;
}
</style>
