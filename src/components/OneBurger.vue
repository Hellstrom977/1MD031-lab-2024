<template>
  <div class="burger-type" > 
  <b class="burger-name">{{burger.name}}</b> <br>   
  <img v-bind:src="burger.img"> 
    <ul>
        <li>{{burger.kCal}} kCal</li>
        <li v-if="burger.lactose">Contains<span class="ingredient"> lactose</span></li>
        <li v-if="burger.gluten">Contains<span class="ingredient"> gluten</span></li>
        <li v-if="burger.goodVibes">Contains <span class="ingredient"> good vibes</span></li>
    </ul>

        <div class="amount-section">
            <span>Amount</span>
            
            <button class="amount-button" v-on:click="decreaseAmount">-</button>
            <span>{{ amountOrdered }} </span>  
            <button class="amount-button" v-on:click="increaseAmount">+</button>
        </div>

    </div>


</template>

<script>


export default {
  name: 'OneBurger',
  props: {
    burger: Object
  },
data: function() {
    return {
        amountOrdered:0
    }
},

methods: {
    increaseAmount: function(){
        this.amountOrdered++
          this.$emit('orderedBurger',
            {name:this.burger.name,
            amount: this.amountOrdered
            }

        );
    },
    decreaseAmount: function(){
        if(this.amountOrdered > 0){
            this.amountOrdered--
            this.$emit('orderedBurger',
            {name:this.burger.name,
            amount: this.amountOrdered
            }

        );
        }

    },

}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>


.amount-section{
    display:flex;
    justify-content: center;
    align-items: center;
}
.amount-button {
    width: 20px;
    height: 20px;
    display:flex;
    justify-content: center;
    align-items: center;
    background-color: white;
    border-radius: 5px;
    font-size: 15px;
    margin:5px;


}

.burger-type{
    
    width:200px;
    /*height:270px;*/
    margin:10px;
    padding:1px;
    vertical-align:top ;
    color:white;
    background-color: rgb(186, 74, 14);
    text-align: center;
    border-width: 5px;
    border-style: outset;
    border-color:rgb(186, 74, 14);
}

.burger-name{
    font-size: 16px;
}

.burger-type ul {
    text-align: left;
    margin-left: -20px;
}

.burger-type img {
    width: 100%;
    height: 180px;
}

.ingredient {
    font-weight: bold;
}
.vegetarian {
    color: green;
    font-weight: bold;
}

</style>