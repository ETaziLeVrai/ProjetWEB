<template>
  <div>
      <md-card>
    <h1>Détail du restaurant {{ id }}</h1>
    <h3>Nom du Restaurant : {{ restaurant.name }}</h3>
    <h3>Cuisine : {{ restaurant.cuisine }}</h3>
    <h3>Ville : {{ restaurant.borough }} </h3>
    <h3>Adresse : {{ restaurant.address.building }} {{ restaurant.address.street }} </h3>
    <h3>Photo du restaurant: </h3>
    <RestaurantImage />
    <h3>Notation : {{ restaurant.grades }} </h3>
    <h3>Emplacement Maps : {{restaurant.address.coord[0]}} </h3>

   
    <div id="app">
    <Map :adress = 'restaurant.address' />
  </div>
      </md-card>
  </div>
</template>

<script>
import Map from './map';
import RestaurantImage from './RestaurantImage';

export default {
  name: 'Restaurant',
  components: {
   Map,
   RestaurantImage
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
      let imgUrl = "https://api.unsplash.com/photos/random/?client_id=PnvDciioIsipuy-DFRWQQmZSP3B18rVjbLRqSmqiknw&page=1&query=restaurant";
      let imgElement = document.querySelector("#unsplashImg");
      let imgLink = document.querySelector("#linkImg");
      fetch(imgUrl)
      .then((response) => response.json())
      .then((jsonData) => {
          imgElement.src = jsonData.urls.regular;
          imgLink.setAttribute("href", jsonData.links.html)
      })
  },
  
  
  methods : {

  },
  
}
</script>

<style>

</style>
