<template>
  <div>
      <b-row>
        <b-col v-for="network in networks" :key="network.id">
          <b-card-group deck>
            <b-card 
            :title="network.company" 
            style="width: 300px; margin-left:40px; margin-top:4%" 
            >
              <br>
              <b-button @click="idNet=network.id, company=network.company, movePag()" variant="primary">Mostrar Estaciones</b-button>
            </b-card>
          </b-card-group>
        </b-col>
      </b-row>
      <Nuxt />
    </div>
  </template>
  
  <script>
  import axios from "axios";
  
  export default {
    layout: "default",
    data(){
      return {
        networks: null,
        stationsNetworks : null,
        idNet : null,
        company : null,
      }
    }, 
  
   beforeMount(){
      this.getTodosNetworks();
      this.getStationsNetworks();
    },
  
    methods:{
      async getTodosNetworks(){
        const url = "http://api.citybik.es/v2/networks";
        const response = await axios.get(url);
        console.log(response);
        this.networks = response.data.networks;
      },
  
      // Obtener los datos del total de bicicletas libres y espacios libres
      async getStationsNetworks(){
        const url = "http://api.citybik.es/v2/networks/";
        const response = await axios.get(url);
        //console.log(response);
        this.stationsNetworks = response.data.networks;
  
      },
  
      movePag(){
        //console.log(this.idNet);
        localStorage.setItem("id", JSON.stringify(this.idNet));
        localStorage.setItem("compañia", JSON.stringify(this.company));
        window.open("estaciones", "_self");
      }
    },
  }
  </script>