<template>
  <div>
   
    <md-card flex="50" >
    <md-card-content>
    <form @submit.prevent="ajouterRestaurant($event)">
        <label>
            Nom : <input name="nom" type="text" required v-model="nom">
        </label>
        <label>
            Cuisine : <input name="cuisine" type="text" required v-model="cuisine">
        </label>

        <button class="button" >Ajouter</button>
    </form>

    <h1>Nombre de restaurants : {{nbRestaurantsTotal}}</h1>
    <p>Chercher par nom : <input 
                            @input="chercherRestaurants()" 
                            type="text" 
                            v-model="nomRestauRecherche"
                          ></p>
    <p>Nb de pages total : {{nbPagesTotal}}</p>
    <p>Nb restaurants par page : 
        <input 
            @input="getRestaurantsFromServer()"
            type="range" min=2 max=1000 v-model="pagesize"
        >{{pagesize}}</p>
    <md-button class="md-raised" :disabled="page===0" @click="pagePrecedente()">Précédent</md-button>&nbsp;&nbsp;
    <md-button class="md-raised" :disabled="page===nbPagesTotal" @click="pageSuivante()">Suivant</md-button>
    &nbsp; Page courante : {{page}}

    <md-dialog-alert
      :md-active.sync="first"
      md-content="Aucun restaurant ne correspond à votre recherche"
      md-confirm-text="Ok" />

    <br>
    </md-card-content>
    </md-card>

    <md-table v-model="restaurants" md-sort="name" md-sort-order="asc" md-card>
        <md-table-row>
            <md-table-head>Nom</md-table-head>
            <md-table-head>Cuisine</md-table-head>
            <md-table-head>Action</md-table-head>
        </md-table-row>
        <md-table-row slot="md-table-row" slot-scope="{ item, index }"
            :style="{backgroundColor:getColor(index)}"
            :class="{bordureRouge:(index === 2)}"
            >
            <md-table-cell md-label="Nom" md-sort-by="name">{{item.name}}</md-table-cell>
            <md-table-cell md-label="Cuisine" md-sort-by="cuisine"> {{item.cuisine}}</md-table-cell>
            <md-table-cell md-label="Ville" md-sort-by="borough"> {{item.borough}}</md-table-cell>
            <md-table-cell md-label="Détail"> <router-link :to="'/Restaurant/' + item._id">[Détail d'un restaurant]</router-link> </md-table-cell>
           <md-table-cell md-label="Supprimer"> <a href="#" @click="supprimerRestaurant(item)"><md-icon>	delete</md-icon>  </a></md-table-cell>
        
        </md-table-row>
    </md-table>
    
  </div>
</template>

<script>
import _ from "lodash";

export default {
  name: 'ListeDesRestaurants',
  data: function() {
    return{
      restaurants: [],
      first: false,
      nom: "",
      cuisine: "",
      nbRestaurantsTotal: 0,
      page: 0,
      pagesize: 10,
      nbPagesTotal: 0,
      msg: "",
      nomRestauRecherche: "",
    }
    },
    mounted() {
      console.log("AVANT AFFICHAGE");
      this.getRestaurantsFromServer();
    },
    methods: {
      pagePrecedente() {
        if (this.page === 0) return;

        this.page--;
        this.getRestaurantsFromServer();
      },
      pageSuivante() {
        if (this.page === this.dernierePage) return;
        this.page++;
        this.getRestaurantsFromServer();
      },
      getRestaurantsFromServer() {
        let url = "http://localhost:8080/api/restaurants?";
        url += "page=" + this.page;
        url += "&pagesize=" + this.pagesize;
        url += "&name=" + this.nomRestauRecherche;

        fetch(url)
          .then((responseJSON) => {
            // arrow functions, conserve le bon "this"
            // la réponse est en JSON, on la convertit avec la ligne suivante
            responseJSON.json().then((resJS) => {
              // Maintenant resJS est un vrai objet JavaScript
              this.restaurants = resJS.data;
              this.nbRestaurantsTotal = resJS.count;
              this.nbPagesTotal = Math.round(
                this.nbRestaurantsTotal / this.pagesize
              );
            });
          })
          .catch(function (err) {
            console.log(err);
          });
      },
      chercherRestaurants: _.debounce(function () {
        // appelée que si on n'a pas tapé de touches pendant un certain délai
        this.getRestaurantsFromServer();
        if(this.nbRestaurantsTotal == 0){
          this.first = true;
        }
      }, 300),
      supprimerRestaurant(r) {
        let url = "http://localhost:8080/api/restaurants/" + r._id;
 if(confirm("Do you really want to delete?")){

        fetch(url, {
            method: "DELETE",
          })
            .then((responseJSON) => {
              responseJSON.json().then((resJS) => {
                // Maintenant res est un vrai objet JavaScript
                console.log(resJS.msg);
                this.msg = resJS.msg;
                // On rafraichit la vue
                this.getRestaurantsFromServer();
              });
            })
            .catch(function (err) {
              console.log(err);
            });
}
      },
      ajouterRestaurant(event) {
        // Récupération du formulaire. Pas besoin de document.querySelector
        // ou document.getElementById puisque c'est le formulaire qui a généré
        // l'événement
   
        let form = event.target;

        // Récupération des valeurs des champs du formulaire
        // en prévision d'un envoi multipart en ajax/fetch
        let donneesFormulaire = new FormData(form);

        let url = "http://localhost:8080/api/restaurants";

        fetch(url, {
          method: "POST",
          body: donneesFormulaire,
        })
          .then((responseJSON) => {
            responseJSON.json().then((resJS) => {
              // Maintenant res est un vrai objet JavaScript
              console.log(resJS.msg);
              this.msg = resJS.msg;
              // On rafraichit la vue
              this.getRestaurantsFromServer();
            });
          })
          .catch(function (err) {
            console.log(err);
          });

        this.nom = "";
        this.cuisine = "";
      },
      getColor(index) {
        return index % 2 ? "white" : "lightGrey";
      },
    },
}
</script>


<style>

.md-card {
    width: 90%;
    margin: 2%;
    display: inline-block;
    vertical-align: top;
    
  }
  .md-card-content {

  }
  .button {
  background-color: #fff; 
  border: none;
  padding:8px 16px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 30px;
  cursor: pointer;
  -webkit-transition-duration: 0.4s; 
  transition-duration: 0.4s;
  box-shadow: 0 3px 1px -2px rgba(0,0,0,.2),0 2px 2px 0 rgba(0,0,0,.14),0 1px 5px 0 rgba(0,0,0,.12);
}

  
</style>
