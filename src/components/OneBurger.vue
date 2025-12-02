<template>
  <div class = "Burger">
     <h4>{{burger.name}}</h4>
          <img v-bind:src="burger.url" width="250">
          <ul class="Allergies">
            <li v-if= "burger.gluten">Gluten</li>
            <li v-if= "burger.lactose">Lactose</li>
          </ul>
     <p>{{burger.kCal}} kcal</p>

     <div class="Orders">
          <button v-on:click="decrease">-</button>
          <button v-on:click="increase">+</button>
          <p>Amount: {{amountOrdered}}</p>
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
          amountOrdered: 0,
      }
  },

methods: {
    increase() {
        this.amountOrdered += 1;
        this.$emit('orderedBurger', { name:   this.burger.name,
                                      amount: this.amountOrdered
                                      });
    },
    decrease() {
        if (this.amountOrdered > 0) {
        this.amountOrdered -= 1;
        this.$emit('orderedBurger', { name:   this.burger.name,
                                      amount: this.amountOrdered
                                      });
        }
    }
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

<style scoped>

.Burgers {
    display: grid;
    grid-gap: 10px;
    grid-template-columns: 300px 300px 300px;
    background-color: ;
    color: green;
}

.Orders button {
    width: 30px;
    height: 30px;
    font-size: 20px;
}

.Allergies {
    font-weight: bold;
}


</style>