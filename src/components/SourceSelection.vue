<template>
<div class="searcheditems">
 		<div class="card">
	        <div class="card-header">
	          <h2>Search Food</h2>
	        </div>

	        <div class="card-body">
	          <form id="foodSearch" novalidate class="form-inline" v-on:submit.prevent="searchFood">
	            <div class="col-12">          
	              <input type="text" id="searchItem" class="form-control"  placeholder="Food Item" ></input>
	              <input type="submit" class="btn btn-primary" value="Search Item">
	            </div>    
	            
	          </form>
	        </div>
	    </div>


		<div class="card">
	        <div class="card-header">
	          <h2>Select the Item</h2>
	        </div>

	        <div class="card-body">
	          <table class="table table-stripped">
	            <thead>
	              <tr>
	                <th>Item</th>
	                <th>Calories</th>
	                <th>Fat</th>
	                <th>Protein</th>
	                <th>Actions</th>
	              </tr>
	            </thead>
	             <tbody>
	              <tr v-for="source in sources">
	                <td>{{ source.fields.item_name }}</td>
	                <td>{{ source.fields.nf_calories }}g</td>  
	                <td>{{ source.fields.nf_total_fat}}g</td>
	                <td> N/A </td>
	                <td> <i class="fa fa-plus-circle" aria-hidden="true" v-on:click="removeItem(food)" ></i></td>
	              </tr>
	            </tbody> 
	          </table>
	        </div>
      </div> 
</div>
</template>

<script>

export default{
	name: 'sourceSelection',

	data (){
		return{
			sources:[],
			source:''
		}
	},



	methods:{
			searchFood: function(){
			var foodcheck = document.getElementById('searchItem').value; 
			this.$http.get('https://api.nutritionix.com/v1_1/search/'+foodcheck+'?fields=item_name%2Citem_id%2Cbrand_name%2Cnf_calories%2Cnf_total_fat&appId=ceb86f68&appKey=0eb843c74dbe45f4ee0c60b3fb299f4d')
			.then(response =>{
				var j = response.data;
				this.sources = j.hits;
				/*console.log(j);
				
				var jt=j.hits;
				console.log(jt.length);
				for (var i = 0; i <= jt.length; i++) {
					this.sources=jt[i].fields;
					console.log( this.sources.item_name );
				};*/

							
				
			});
		}	
    }
}

</script>