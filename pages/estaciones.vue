<template>
  <div>
    <center>
      <h1>{{compañia}}</h1>
    </center>
    <br>
    <center>
        <div>
            <b-card no-body class="overflow-hidden" style="max-width: 800px;">
                <b-row no-gutters>
                <b-col md="6">
                    <b-card-img src="https://img.freepik.com/vector-gratis/bicicleta-transporte-ecologico-mundo_188544-2610.jpg" alt="Image" class="rounded-0"></b-card-img>
                </b-col>
                <b-col md="6">
                    <b-card-body :title=compañia>
                        <b-card-text>
                            <h6> Nombre de la red: </h6>
                            <h6> Nombre de la empresa: </h6>
                            <h6> Pais: </h6>
                            <h6> Total de bicicletas libres: </h6>
                            <h6> Total de espacios libres: </h6>
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
            <td v-else>1</td>
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
      compañia: null,
      stations : [],
      bicLibres: null,
      spaceLibres:null,
      total_espacios:null,
    }
  },

  beforeMount(){
  this.getStations();
  },

  methods:{
    async getStations(){
      const id = JSON.parse(localStorage.getItem("id"));
      const compani = JSON.parse(localStorage.getItem("compañia"));
      // COMO LE PASO EL PARAMETRO DEL ID A LA URL?
      const url = `http://api.citybik.es/v2/networks/${id}`;
      const response = await axios.get(url);
      console.log(response);
      this.stations = response.data.network.stations;
      this.compañia = compani;
      this.id = id;
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