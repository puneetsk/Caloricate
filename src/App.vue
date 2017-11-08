<template>
  <div id="app" class="container">
    <div class="page-header">
      <h1>Welcome to Calorie Counter</h1>
    </div>

    <div class="panel panel-default">
      <div class="panel-heading">
        <h2>Add Food</h2>
      </div>

      <div class="panel-body">
        <form id="form" class="form-inline" v-on:submit.prevent="addFood">
          <div class="form-group">
            <label for="fooditem">Food Item </label>
            <input type="text" id="footitem" class="form-control" v-model="newFood.Item"></input>
          </div>

          <div class="form-group">
            <label for="foodcalorie">Calories </label>
            <input type="text" id="foodcalorie" class="form-control" v-model="newFood.Calories"></input>
          </div>

          <div class="form-group">
            <label for="foodfat">Fat </label>
            <input type="text" id="foodfat" class="form-control" v-model="newFood.Fat"></input>
          </div>

          <div class="form-group">
            <label for="foodprotein">Protein</label>
            <input type="text" id="foodprotein" class="form-control" v-model="newFood.Protein"></input>
          </div>

          <input type="submit" class="btn btn-primary" value="Add Food Items">
        </form>
      </div>
    </div>
    <div class="panel panel-default">
      <div class="panel-heading">
        <h2>Calculated Calories</h2>
      </div>

      <div class="panel-body">
        <table class="table table-stripped">
          <thead>
            <tr>
              <th>Item</th>
              <th>Calories</th>
              <th>Fat</th>
              <th>Protien</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="food in Foods">
              <td>{{ food.Item }}</td>
              <td>{{ food.Calories}}g</td>  
              <td>{{ food.Fat }}g</td>
              <td>{{ food.Protein }}g</td>
              <td> <span class="glyphicon glyphicon-trash" v-on:click="removeItem(food)"></span></td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import Firebase from 'firebase'

let config = {
    apiKey: "AIzaSyD9Ghr-J33f8sQ9xB7yLFN9vvQpdgVZ1s0",
    authDomain: "caloricate.firebaseapp.com",
    databaseURL: "https://caloricate.firebaseio.com",
    projectId: "caloricate",
    storageBucket: "caloricate.appspot.com",
    messagingSenderId: "108277077998"
  }

let app = Firebase.initializeApp(config); /* To intialize the Firsebase Connection with the config*/
let db = app.database(); /* Open the database connection */
let dataref= db.ref('Foods'); /* Referencing the category*/


  export default {
    name: 'app', 
    firebase:{
      Foods : dataref /* Exporting the information to the template Food in Foods*/
    },

    data(){
        return{
          newFood : {
            Item : '',
            Calories:'',
            Fat:'',
            Protein:''
          }
        }
    },

    methods:{
      addFood:function(){
        dataref.push(this.newFood);
        this.newFood.Item ="";
        this.newFood.Calories = "";
        this.newFood.Fat="";
        this.newFood.Protein="";
      },

      removeItem:function(food){
        dataref.child(food['.key']).remove();
      }
    } 
  }     

</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

table td, th{text-align: center;}
.btn{margin: 20px;}
</style>
