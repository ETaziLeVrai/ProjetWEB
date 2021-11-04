<template>
  <div id="app">
    <md-tabs  md-alignment="centered" >
      <md-tab  id="tab-home" md-label="Home" to="/"></md-tab>
    <md-tab id="tab-pages" md-label="Restaurant" to="/Restaurant"></md-tab>
    </md-tabs>
    <br>
   
    <router-view></router-view> 
  </div>

  
</template>
<script>
export default {
  name: 'App',
  components: {
  }
}

</script>

<style>
#app {
  /*background: fixed url(https://www.wallpaperbetter.com/wallpaper/63/966/426/vintage-restaurant-1080P-wallpaper.jpg);
 */ font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  /*color: #2c3e50;*/
}
.md-tabs {
  margin-bottom: 0px;
 
}
.md-tabs.md-theme-default.md-primary .md-tabs-navigation {
    background-color: #000000;
}
.md-table {
    margin-left: 10%;
    margin-right: 10%;
    margin-top: 2%;
  
}
#image{
  width: 70%;
    height: 500px;
     display: block;
  margin-left: auto;
  margin-right: auto;
}


</style>
