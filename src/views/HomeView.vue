<template>
 
  <header> 

        <h1> Welcome to BurgerGOAT</h1>
        <h3>Greatest Burger of All Time </h3>
        <img git class="header-image" src="/Bilder/Fire2.jpeg">
    </header>
    <main>
        <section id="burgers-section">
            <p class="select-burger-title"> Select burger: </p>
                <p> This is where you choose your burger</p>
                <br>
          <div id="burger-image-field">

            <Burger
                v-for="burger in burgers"
                v-bind:burger="burger"
                v-bind:key="burger.name"
                v-on:orderedBurger="addToOrder($event)"
                />
          </div>
        </section>
        

        <section id="order-info">
                <p class="customer-info-title">Customer information:</p>
                <p>These are perhaps the greatest burgers of all time. If you don't agree, you're wrong!</p>
        

            <section id="contact"> 
                <div class="delivery-info-title"> Delivery information: </div>
                <form>
                    <div>
                        <label for="Fullname">Full name </label><br>
                        <input type="text" id="Fullname" v-model="fullName" required="required" placeholder="First- and Last name">
                    </div>
                    <div>
                        <label for="Email"> E-mail</label><br>
                        <input type="text" id="Email" v-model="email" required="required" placeholder="E-mail adress">
                    </div>
        

                    <div> 
                        <label for="Payment">Payment options</label><br>
                        <select id="payment" v-model="payment" required = "required"  >
                            <option>Credit card</option>
                            <option>Klarna</option>
                            <option>Swish</option>
                            <option>Krypto</option>
                        </select>
                    </div>
                    <p>Please indicate point of delivery: </p>
                    <div id="map-container">
                        <div id="map" v-on:click="setLocation">
                            <div id="location-dot"
                                v-bind:style="{left:location.x + 'px',  top:location.y +'px'}">
                                T
                            </div>
                        </div>
                            


                    </div>
                    <fieldset>
                        <legend>Select gender</legend>
                        <div>
                            <input type="radio" id="male" v-model="gender" value="male"> 
                            <label for="male">Male</label>
                        </div>
                        <div>
                            <input type="radio" id="female" v-model="gender" value="female">
                            <label for="female">Female</label>
                        </div>
                        <div>
                            <input type="radio" id="non-binary" v-model="gender" value="non-binary">
                            <label for="non-binary">Non-Binary</label>
                        </div>
                        <div>
                            <input type="radio" id="Undiscloded" v-model="gender" value="Undisclosed" checked="checked">
                            <label for="Undiscloded">Undisclosed</label>
                        </div>
                            
                    </fieldset>
                
                    

                </form>
                <button type="submit" class="submit-button" v-on:click="placeOrder">
                    <img src="/Bilder/Green_check.svg.png" style="width: 15px;">
                    Place my order!
                </button>
            </section>
        </section>

        

    </main>
    <hr> 
    <footer>
        End notes
    </footer>
   
<div>

</div>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'


const socket = io("localhost:3000");

function MenuItem(name, img, kCal, gluten, lactose ){
  this.name = name;
  this.img = img;
  this.kCal = kCal;
  this.gluten = gluten;
  this.lactose = lactose;
  this.goodVibes = goodVibes;
}




export default {
  name: 'HomeView',
  components: {
    Burger
  },
  
  data: function () {
    return {
      burgers: menu ,
      fullName:'',
      email:'',
      street: '',
      houseNumber:'',
      payment:'',
      gender:'',
      orderedBurgers: {},
      location: {x: 0, y: 0}

    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    
    addToOrder: function(event){
        this.orderedBurgers[event.name]=event.amount;
    },
    placeOrder: function(){

       socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: this.location.x, y: this.location.y },
                                orderItems: this.orderedBurgers,
                                name: this.fullName,
                                email:this.email,
                                gender: this.gender,
                                payment: this.payment

                              }
        
                 );  

        console.log('Order is sent!' ,{
        fullName: this.fullName,
        email: this.email,
        /*street: this.street,
        houseNumber: this.houseNumber,*/
        payment: this.payment,
        gender: this.gender,
        orderedBurgers: this.orderedBurgers,
        location: this.location
        
        
    })
        
  },
  setLocation: function(event){
    var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
        const x = event.clientX - 10 - offset.x;
        const y = event.clientY - 10 - offset.y;
        this.location.x= x;
        this.location.y= y;
        
  }
}
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Agbalumo&family=Cormorant:wght@700&display=swap');




header {
    /*background-image: url("/Bilder/Fire2.jpeg");*/
    /*background-image: url("../img/polacks.jpg");*/
    background-size: cover;
    overflow: hidden;
    text-align: center;
    max-width: 100rem;
    height: 160px;
    
    margin-left: 4px;
    margin-right: 4px;
    position: relative;

}

.header-image {
    width: 100%;
    height: auto;
    opacity: 0.75;
    position: absolute;
    
    left: 0;
    z-index: 1;

}
header h1 {
    
    text-align: center;
    font-family: 'Agbalumo';
    font-size: 50px;
    color: rgb(23, 21, 18);
    margin:0;

    text-shadow: 3px 3px 4px rgb(74, 70, 70);
    left:13%;
    top:10px;
    position: absolute;
    z-index: 10;


}
header h3 {
    
    margin: 0 auto;
    text-align: center;
    font-size: 27px;
    color: rgb(23, 21, 18);

    text-shadow: 3px 3px 4px rgb(74, 70, 70);
    left:29%;
    top:80px;
    position: absolute;
    z-index: 10;

}


#burgers-section {
    background-color: black;
    color:white;
    padding: 3px 15px;
    margin-left: 4px;
    margin-right: 4px;
    border: 3px dotted white;
}

#order-info {
    background-color: rgb(206, 84, 18);
    padding: 3px 15px;
    margin-left: 4px;
    margin-right: 4px;
    border:3px dotted white;
    color:black;

}

.select-burger-title {
    font-size: 20px;
    font-weight: bold;
    color:white;
 
}
#burger-image-field {
    display: grid;
    grid-gap: 10px;
    grid-template-columns: repeat(auto-fit,minmax(220px, 220px));
    justify-content: center;

    margin: 0 auto;
    text-align: center;

}


.burger-info {
    color:white;
}
.customer-info-title {
    font-size: 20px;
    font-weight: bold;
}
.delivery-info-title {
    font-size: 20px;
    font-weight: bold;
    margin-bottom: 5px;
}
#contact div{
    margin-bottom:10px;
}


button:hover {
    background-color: green;
    cursor:pointer;
}
.submit-button {
    background-color: rgb(247, 241, 241);
    color:rgb(10, 10, 10);
    border-width: 5px;
    border-color: grey;
    border-radius: 10px;
    box-shadow: 2px 2px dark rgb(48, 41, 41);
    padding: 2px 8px;
    margin-top:30px;
    margin-bottom:10px;

}

#map-container{
    width: 100%;
    height: 400px;
    overflow: scroll;
    border: 3px;
    border-color: black;
    border-style: solid;
}

  #map {
    position: relative;
    width: 1920px;
    height: 1078px;
    background: url("../img/polacks.jpg");
    background-size: cover;
    background-position: center;
  }
  #location-dot{
    position:absolute;
    width: 20px;
    height: 20px;
    background-color: black;
    border-radius: 10px;
    color:white;
    text-align: center;
    font-size: 18px;
    
  }
</style>