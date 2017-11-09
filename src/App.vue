<template>
  <div id="app" class="container-fluid">



      <header>
      <div class="collapse bg-dark" id="navbarHeader">
        <div class="container">
          <div class="row">
            <div class="col-sm-8 py-4">
              <h4 class="text-white">About</h4>
              <p class="text-muted">Add some information about the album below, the author, or any other background context. Make it a few sentences long so folks can pick up some informative tidbits. Then, link them off to some social networking sites or contact information.</p>
            </div>
            <div class="col-sm-4 py-4">
              <h4 class="text-white">Contact</h4>
              <ul class="list-unstyled">
                <li><a href="#" class="text-white">Follow on Twitter</a></li>
                <li><a href="#" class="text-white">Like on Facebook</a></li>
                <li><a href="#" class="text-white">Email me</a></li>
              </ul>
            </div>
          </div>
        </div>
      </div>
      <div class="navbar navbar-dark bg-dark">
        <div class="container d-flex justify-content-between">
          <a href="#" class="navbar-brand"><img src="./assets/drink.png" class="img-responsive" width="60"></a>
          <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarHeader" aria-controls="navbarHeader" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
          </button>
        </div>
      </div>
    </header>



    <div class="container">    
      <div class="card">
        <div class="card-header">
          <h2>Add Food</h2>
        </div>

        <div class="card-body">
          <form id="needs-validation" novalidate class="form-inline" v-on:submit.prevent="addFood">
            <div class="col">          
              <input type="text" id="footitem" class="form-control" v-model="newFood.Item" placeholder="Food Item" ></input>
            </div>

            <div class="col">
             
              <input type="text" id="foodcalorie" class="form-control"  v-model="newFood.Calories" placeholder="Calorie in g" ></input>
            </div>

            <div class="col">
             
              <input type="text" id="foodfat" class="form-control"  v-model="newFood.Fat"  placeholder="Fat in g" ></input>
            </div>

            <div class="col">              
              <input type="text" id="foodprotein" class="form-control"  v-model="newFood.Protein"  placeholder="Protein in g" ></input>
            </div>

            <input type="submit" class="btn btn-primary" value="Add Food Items">
          </form>
        </div>
      </div>
   
      <div class="card">
        <div class="card-header">
          <h2>Calculated Calories</h2>
        </div>

        <div class="card-body">
          <table class="table table-stripped">
            <thead>
              <tr>
                <th>Item</th>
                <th>Calories</th>
                <th>Fat</th>
                <th>Protien</th>
                <th>Actions</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="food in Foods">
                <td>{{ food.Item }}</td>
                <td>{{ food.Calories}}g</td>  
                <td>{{ food.Fat }}g</td>
                <td>{{ food.Protein }}g</td>
                <td> <i class="fa fa-minus-circle" aria-hidden="true" v-on:click="removeItem(food)" ></i></td>
              </tr>
            </tbody>
          </table>
        </div>
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
body{background: #7BB0A6;}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  
}
.page-header{color: #fff;}
table td, th{text-align: center;}
.btn{margin: 20px;}
.container-fluid{
  margin: 0; padding: 0;
}
</style>
