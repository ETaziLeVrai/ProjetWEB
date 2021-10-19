<template>
  <div>
      <md-card>
    <h1>Détail du restaurant {{ id }}</h1>
    <h3>Nom du Restaurant : {{ name }}</h3>
    <h3>Cuisine : {{ cuisine }}</h3>
    <h3>Ville : </h3>
    <h3>Photo du restaurant: </h3>
    <h3>Notation : </h3>
    <h3>Emplacement Maps : </h3>
      </md-card>
  </div>
</template>

<script>
export default {
  name: 'Restaurant',
  props: {
  },
  computed: {
      id() {
          return this.$route.params.r
      }
  },
  data: function() {
      return {
          restaurant: null
      }
  },
  mounted() {
      console.log("ID : " + this.id)
      let url = "http://localhost:8081/api/restaurants/" + this.id;
      fetch(url)
      .then(reponse => {
          return reponse.json();
      }).then(data => {
          this.restaurant = data.restaurant;
      })   
  },
  methods : {
  }
}
</script>

<style>

</style>
