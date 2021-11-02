<template>
  <div>
      <md-card>
    <h1>Détail du restaurant {{ id }}</h1>
    <h3>Nom du Restaurant : {{ restaurant.name }}</h3>
    <h3>Cuisine : {{ restaurant.cuisine }}</h3>
    <h3>Ville : {{ restaurant.borough }} </h3>
    <h3>Adresse : {{ restaurant.address.building }} {{ restaurant.address.street }} </h3>
    <h3>Photo du restaurant: </h3>
    <h3>Notation : {{ restaurant.grades }} </h3>
    <h3>Emplacement Maps : {{restaurant.address.coord[0]}} </h3>
   
    <div id="app">
    <Map/>
  </div>
      </md-card>
  </div>
</template>

<script>
import Map from './map';

export default {
  name: 'Restaurant',
  components: {
   Map
 },
  props: {
  },
  computed: {
      id() {
          console.log(this.$route.params.r);
          return this.$route.params.r
      },
 
  },
  data: function() {
      return {
          restaurant: null
      }
  },
  mounted() {
      console.log("ID : " + this.id)
      let url = "http://localhost:8080/api/restaurants/" + this.id;
      fetch(url)
      .then(reponse => {
          return reponse.json();
      }).then(data => {
          this.restaurant = data.restaurant;
      })   
  },
  
  
  methods : {

  },
  
}
</script>

<style>

</style>
