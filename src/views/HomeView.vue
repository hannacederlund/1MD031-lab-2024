<template>
<header>
        <h1>Welcome to BurgerOnline!</h1>
    </header>
<main>
    <br />
    <section id="BurgerSelect">
        <h3>Select your burger</h3>
        <p>Here you can choose burgers after you liking!</p>
        <div class="Burgers">
                <Burger v-for="burger in burgers"
                     v-bind:burger="burger"
                     v-bind:key="burger.name"
                     v-on:orderedBurger="addToOrder($event)"
                />
        </div>
    </section>
    <section id="ContactInfo">
        <h3>Contact information</h3>
        <p> Write your contact information here ...</p>
        <form>
            <p>
                <label for="firstname">First name</label><br>
                <input type="text" id="firstname" v-model="FirstName" required="required" placeholder="First name">
            </p>
            <p>
                <label for="lastname">Last name</label><br>
                <input type="text" id="lastname" v-model="LastName" placeholder="Last name">
            </p>
            <p>
                <label for="email">Email</label><br>
                <input type="email" id="email" v-model="email" required="required" placeholder="E-mail address">
            </p>
            <p>
                <label for="payment">Payment Options</label><br>
                <select id="payment" v-model="paymentOptions">
                    <option selected>Credit Card</option>
                    <option>Cash</option>
                    <option>Swish</option>
                </select>
            </p>
            <p>
                <label>Gender:</label><br>

                <input type="radio" id="male" v-model="gender" value="male">
                <label for="male">Male</label><br>

                <input type="radio" id="female" v-model="gender"  value="female">
                <label for="female">Female</label><br>

                <input type="radio" id="none" v-model="gender"  value="none" checked>
                <label for="none">None</label><br>
            </p>
        </form>
    </section>

    <div id="map-container">
        <div>
            <div id="map" v-on:click="setLocation">
                <div class="target"
                    v-bind:style="{ left: location.x + 'px',
                                    top: location.y + 'px' }">
                </div>
            </div>
        </div>
    </div>

    <div style="text-align: center; margin-top: 30px;">
        <button type="submit" id="orderButton" v-on:click="sendOrder"
         style="background-color: white; border: 2px solid; padding:10px 20px; border-radius: 10px;
         cursor: pointer; gap: 10px; font-size: 16px; display: inline-flex; align-items: center;
         gap; 10px">
            <img src="/img/CheckButton.png" alt="Send Order" width ="30" height="30">
            Send info
        </button>
    </div>
    <meta charset="utf-8"/>
    </main>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io("localhost:3000");

const MenuItem = {
    Name: "name",
    url: "url",
    kcal: "kcal",
    gluten: "gluten",
    lactos: "lactos",
}

//let burgersData = [
//      {name: "CheeseBurger", kCal: 300, url: "img/Burger3.png", lactose: true, gluten: true },
//      {name: "ChipsBurger", kCal: 400, url: "img/Burger2.png", lactose: false, gluten: true },
//      {name: "DoubleCheese", kCal: 500, url: "img/Burger1.png", lactose: false, gluten: true }
//      ];


export default {
  name: 'HomeView',
  components: {
    Burger
  },

  data: function () {
    return {
      burgers: menu,

      FirstName: "",
      LastName: "",
      email: "",
      paymentOptions: "Credit Card",
      gender: "none",
      orderedBurgers: {},

      location: { x: 0,
                  y: 0
                }
    }
  },


  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};

      var x = event.clientX - 10 - offset.x;
      var y = event.clientY - 10 - offset.y;

      this.location.x = x;
      this.location.y = y;
    },

    setLocation: function(event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                     y: event.currentTarget.getBoundingClientRect().top};

      var x = event.clientX - 10 - offset.x;
      var y = event.clientY - 10 - offset.y;

      this.location.x = x;
      this.location.y = y;
    },

    addToOrder: function (event) {
      this.orderedBurgers[event.name] = event.amount;
    },

    sendOrder: function () {
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: {x: this.location.x,
                                          y: this.location.y,
                                          FirstName: this.FirstName,
                                          LastName: this.LastName,
                                          Email: this.email,
                                          PaymentOption: this.paymentOptions,
                                          Gender: this.gender,
                                },
                                orderItems: this.orderedBurgers
      });
    }
  }
}
</script>

<style>

  #map-container {
    width: 600px;
    height: 400px;
    overflow:scroll;
    border: 1px solid black;
    margin: 30px;
  }

  #map {
    width: 1920px;
    height: 1078px;
    background: url("/img/polacks.jpg");
    position: relative;
  }

  .target {
    position: absolute;
    width:20px;
    height:20px;
    background: black;
    border-radius: 10px;
  }


    @import url('https://fonts.googleapis.com/css2?family=Agbalumo&family=Cormorant:wght@700&display=swap');
    body {
        font-size: 18pt;
        font-family: roboto;
    }

    p {
        color: Green;
    }

    h1 {
        font-family: 'arial';
        font-size: 36pt;
    }
    main, header, footer, nav ul {
        max-width: 80rem;
        margin: 0 auto 0 auto;
    }
    main {
        background-color: OliveDrab;
    }

    /* nav ul li {
        display: inline-block;
        background-color: grey;
        padding: 1em;
        margin: 1em;
    } */

    header {
        background-image: url("../img/RestaurangBild.webp");
        background-size: cover;
        background-repeat: no-repeat;
        background-position: center;
        overflow: hidden;
        width: 100%;
        height: 200px;
        opacity: 0.8;
        color: Black;
    }

    header h1 {
        width:80rem;
        margin: 0 auto;
        text-align: center;
    }

    nav ul {
        display: grid;
        grid-template-columns: repeat(auto-fill, 9.25em);
        gap: 1em;
        padding: 0;
    }

    nav li {
        display: block;
        background-color: grey;
        padding: 1em;
    }

    section {
        margin: 10px 5px;
        padding: 10px;
    }

    .Very-good {
        color: green;
    }

    .Master {
        color: green;
        font-weight: bold;
    }

    .Allergies {
        color:#ffx5500;
    }

    .Allergies {
        font-weight: bold;
    }

    #BurgerSelect {
        background-color: black;
        color: white;
        margin: 30px;
        border: 2px dashed white;
    }

    .Burgers {
        display: grid;
        grid-gap: 10px;
        grid-template-columns: 300px 300px 300px;
        background-color: ;
        color: green;
    }


    .Burger {
         /*margin: 30px;*/
         border-radius: 20px;
         padding: 20px;
         font-size: 100%;
         color: white;
         background-color: black;
    }

    #ContactInfo {
        background-color: white;
        color: black;
        margin: 30px;
        border: 2px dashed black;
    }

    #orderButton:hover {
        background-color: black;
        color: green;
        cursor: pointer;
        cursor: white;
    }

    #orderButton {
        margin: 20px 5px
    }

    @media screen and (max-width: 800px) {
        h1 {
            font-size: 6vw;
        }
    }


</style>