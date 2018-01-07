<template>
  <div class="header-box">
	  <div class="table-row header">
	  	<h1>Name</h1>
	    <h1>Cost</h1>
	    <h1>Markup</h1>
	    <h1>Price</h1>
	  </div>
	  <div v-if="lineItem.id !== undefined" v-for="lineItem in lineItems" class="table-row">
		  <p>{{lineItem.itemName}}</p>
		  <p>${{lineItem.itemCost}}</p>
		  <p>{{lineItem.itemMarkup}}%</p>
		  <p>${{lineItem.itemPrice}}<button v-on:click="removeLineItem(lineItem.id)" class="remove">X</button></p>
	  </div>
	  <div v-if="lineItems.length > 2" class="table-row">
	  	<p style="font-weight: bold;">Totals</p>
	  	<p>${{totalCost}}</p>
	  	<p>${{totalMarkup}}</p>
	  	<p>${{totalPrice}}</p>
	  </div>
	  <div class="table-row">
	  	<input type="text" placeholder="item name" v-model="name" required />
	  	<input type="number" placeholder="item cost" v-model="cost" required />
	  	<input type="number" placeholder="item markup" v-model="markup" required />
	  	<p> {{calcPrice()}} </p>
	  </div>
	  <button v-on:click="addLineItem" class="add-item">Add Line Item</button>  
  </div>
</template>

<script>

let nextLineItemId = 1

  export default {

    data() {
      return {
      	name: "",
      	cost: "",
      	markup: "",
      	totalPrice: 0,
      	totalCost: 0,
      	totalMarkup: 0,
		    lineItems: [
		    	{
		    	}
		    ]
    }
   },
    methods:{
    	addLineItem(){
    		const trimmedName = this.name.trim()
    		const trimmedCost = this.cost.trim()
    		const trimmedMarkup = this.markup.trim()
    		const price = parseFloat((this.cost * (1+(this.markup/100)))).toFixed(2)
    		if (trimmedName && trimmedCost && trimmedMarkup){
    			this.totalPrice = this.totalPrice += Number(price)
    			this.totalCost = this.totalCost += Number(trimmedCost)
    			this.totalMarkup = this.totalMarkup += (trimmedCost * (trimmedMarkup/100))
    			this.lineItems.push({
    				id: nextLineItemId++,
    				itemName: trimmedName,
    				itemCost: trimmedCost,
    				itemMarkup: trimmedMarkup,
    				itemPrice: price
    			})
    			this.name = "",
    			this.cost = "",
    			this.markup = ""
    		}
    	},
	    removeLineItem(idToRemove){
	    	this.lineItems = this.lineItems.filter(lineItem => {
	    		console.log (lineItem.id)
	    		console.log (lineItem.itemCost)
	    		console.log (lineItem.itemName)
	    		console.log (lineItem.itemMarkup)
	    		console.log (this.totalCost)
	    		this.totalCost -= (Number(lineItem.itemCost))
	    		return lineItem.id !== idToRemove
	    	})
	    },
	    calcPrice(){
	    	return "$" + (parseFloat(this.cost * (1+(this.markup/100)))).toFixed(2);
	    }
  	},
}
</script>

<style>
	@import '../assets/generalstyles.css'
</style>