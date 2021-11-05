# ProjetWEB
Projet Javascript M1 Miage


Ce mini projet a pour but de gérér une liste des restaurants. Chaque restaurant possède un nom, 
un type de cuisine, une adresse, une notation.

Nous pouvons faire une recherche par nom sur les restaurants,
ajouter un nouveau restaurant, supprimer un restaurant de la liste.

## Technologies utilisées : 
Vuejs

Material UI

MongoDB

Leaflet

## Démarrer le projet :
Il faut tout d'abord lancer le serveur avec la commande :
```bash
node serverCrudWithMongo.js
```
Le serveur doit utiliser le port 8080. Dans  ce serveur  des données sont situées dans MongoDb, via des WebServices.

Ensuite connecter le client avec la commande :
```bash
npm run serve
```

Afin de pouvoir présenter la position de chaque restaurant sur une Map, nous avons utilisé Leaflet qui est un framework javascript.

Pour installer Leaflet nous avons utilisé ces deux commandes : 


```bash
npm install --save leaflet
```

```bash
npm install --save vue2-leaflet
```

