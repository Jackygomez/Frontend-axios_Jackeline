<template>
  <div>
    <br>
    <center>
        <div>
            <b-card no-body class="overflow-hidden" style="max-width: 800px;">
                <b-row no-gutters>
                  <b-col md="6">
                      <b-card-img src="https://img.freepik.com/vector-gratis/bicicleta-transporte-ecologico-mundo_188544-2610.jpg" alt="Image" class="rounded-0"></b-card-img>
                  </b-col>
                  <b-col md="6">
                      <b-card-body>
                          <b-card-text>
                              <h2> {{red}} </h2>
                              <br>
                              <h6> Nombre de la empresa: {{Array.isArray(infoNetwork.company)?infoNetwork.company[0]:infoNetwork.company}} </h6>
                              <!--HAY UN BUG AQUI, SALE ERROR DE COUNTRY O CITY-->
                              <!--<h6> Pais: {{infoNetwork.location.country}} ({{infoNetwork.location.city}}) </h6>-->
                              <br>
                              <h6> Total de bicicletas libres: {{bicLibres}}  </h6>
                              <br>
                              <h6> Total de espacios libres: {{spaceLibres}} </h6>
                          </b-card-text>
                      </b-card-body>
                  </b-col>
                </b-row>
            </b-card>
        </div>
    </center>
    <br>
    <table id="firstTable">
        <thead>
            <tr>
              <th>Nombre de la Estación</th>
              <th>Fecha de la Actualización</th>
              <th>Bicicletas Libres</th>
              <th>Espacios Libres</th>
              <th>Total de Espacios</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="station in stations" :key="station.id">
              <td>{{station.name}}</td>
              <td>{{station.timestamp}}</td>
              <td>{{station.free_bikes}}</td>
              <td v-if="station.empty_slots == null">0</td>
              <td v-else>{{station.empty_slots}}</td>
              <td>{{station.free_bikes + station.empty_slots}}</td>
            </tr>
        </tbody>
    </table>
    <Nuxt />
  </div>
</template>

<script>
import axios from "axios";

export default {
  layout: "default",
  data() {
    return {
      id:null,  
      red: null,
      infoNetwork: [],
      stations : [],
      bicLibres: null,
      spaceLibres:null,
    }
  },

  beforeMount(){
  this.getStations();
  },

  methods:{
    async getStations(){
      const id = JSON.parse(localStorage.getItem("id"));
      const name = JSON.parse(localStorage.getItem("red"));
      // COMO LE PASO EL PARAMETRO DEL ID A LA URL?
      const url = `http://api.citybik.es/v2/networks/${id}`;
      const response = await axios.get(url);
      console.log(response);
      this.infoNetwork = response.data.network;
      this.stations = response.data.network.stations;
      this.red = name;
      this.id = id;

      const datos = response.data.network.stations;

      var contador_bikes = null;
      var contador_space = 0;

      for (const n in datos ) {
        //console.log(response.data.network.stations[info].empty_slots);
        //console.log(response.data.network.stations[info].free_bikes);
        contador_bikes += response.data.network.stations[n].free_bikes
        if (response.data.network.stations[n].empty_slots != null){
          contador_space += response.data.network.stations[n].empty_slots
        } 
      }
      this.bicLibres = contador_bikes;
      this.spaceLibres = contador_space;
      console.log(contador_bikes);
      console.log(contador_space);
    },
  },
};
</script>

<style>
  table {
  font-family: 'Open Sans', sans-serif;
  width: 99%;
  border-collapse: collapse;
  border: 3px solid #44475C;
  margin: 10px 10px 0 10px;
}

table th {
  text-transform: uppercase;
  text-align: left;
  background: #44475C;
  color: #FFF;
  padding: 8px;
  min-width: 30px;
}

table td {
  text-align: left;
  padding: 8px;
  border-right: 2px solid #7D82A8;
}
table td:last-child {
  border-right: none;
}
table tbody tr:nth-child(2n) td {
  background: lavender;
}
</style>