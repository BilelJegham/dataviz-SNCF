<template>
  <div id="app">
    <MapPerdu :data="filteredItems" :gares="gares"/>
    <MapPerduRetrouve :data="filteredItems" :gares="gares"/>
    <MapFre :data="filteredItems" :gares="gares"/>
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
    });
   d3.csv(
        "./referentiel-gares-voyageurs.csv"
    ).then(function(data) {
      t.gares= data
    });

  },
  computed: {
      filteredItems() {
        return this.data.filter((thing, index, self) =>
          index === self.findIndex((t) => (
            t.CODE_UIC === thing.CODE_UIC 
          )))
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
}
html,
body, h3 {
  margin: 0;
  padding: 0;
}
</style>