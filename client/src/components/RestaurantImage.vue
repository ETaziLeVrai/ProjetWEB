<template>
    <div>
        <a id="linkImg" href="#">
            <img style="height:500px" class="center" alt="" id="unsplashImg"/>
        </a>
    </div>
</template>

<script>

export default {
  components: {
 },
  props: {
  },
  mounted() {
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

.center {
  display: block;
  margin-left: auto;
  margin-right: auto;
}

</style>
