<template>
  <div>
      <md-card  >
    <md-card-content>
    <p id="title">{{ restaurant.name }}</p>
   
   <RestaurantImage /> 
 
    <p>Cuisine : {{ restaurant.cuisine }}</p>
    <p> Ville : {{ restaurant.borough }} </p>
    <p><md-icon>location_on</md-icon>Adresse : {{ restaurant.address.building }} {{ restaurant.address.street }} </p>
    <p> Notation : {{this.result}} </p>
    
     </md-card-content>
     </md-card>
    <Map :adress = 'restaurant.address' />
      
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
          restaurant: null,
          sum: null,
          result: null,
          
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
        for( var i = 0; i < this.restaurant.grades.length; i++ ){
            this.sum += parseInt( this.restaurant.grades[i].score, 10 ); 
        }
        this.result =this.sum/this.restaurant.grades.length;
      })
  },
  
  
  methods : {
 
  },
}
  
</script>

<style>
#title {
    font: italic 1.2em "Fira Sans", serif;
    text-align: center;
    font-size:25px;
}
p {
    text-align: center;
    font: italic 1.2em "Fira Sans", serif;
}

</style>
