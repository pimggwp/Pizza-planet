<template>
  <div class="row">
    <div class="col-xs-12 col-lg-6">
      <table class="table table-hover">
        <thead class="thead-default">
          <tr>
            <th>Size</th>
            <th>Price</th>
            <th>Add to basket</th>
          </tr>
        </thead>
        <tbody v-for="item in getMenuItems" :key="item['.key']">
          <tr>
            <td>
              <strong>{{item.name}}</strong>
            </td>
          </tr>
          <tr v-for="option in item.options" :key="option.price">
            <td>{{option.size}}</td>
            <td>{{option.price | currency}}</td>
            <td>
              <button
                class="btn btn-sm btn-outline-success"
                type="button"
                @click="addToBasket(item,option)"
              >+</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <!-- Shopping basket -->
    <div class="col-xs-12 col-lg-6">
      <div v-if="basket.length > 0">
        <table class="table">
          <thead class="thead-default">
            <tr>
              <th>Quantity</th>
              <th>Item</th>
              <th>Total</th>
            </tr>
          </thead>
          <tbody v-for="item in basket">
            <tr>
              <td>
                <button class="btn btn-sm" type="button" @click="decreaseQuantity(item)">-</button>
                <span>{{item.quantity}}</span>
                <button class="btn btn-sm" type="button" @click="increaseQuantity(item)">+</button>
              </td>
              <td>{{item.name}} {{item.size}}"</td>
              <td>{{item.price * item.quantity | currency}}</td>
            </tr>
          </tbody>
        </table>
        <p>Order total: {{total | currency}}</p>
        <button class="btn btn-success btn-block" @click="addNewOrder">Place order</button>
      </div>
      <div v-else>
        <p>{{basketText}}</p>
      </div>
    </div>
  </div>
</template>

<script>
import { mapGetters } from "vuex";
import { dbMenuRef, dbOrdersRef } from "../firebaseConfig";
export default {
  data() {
    return {
      basket: [],
      basketText: "Your basket is empity!"
    };
  },
  computed: {
    // getMenuItems() {
    //   return this.$store.getters.getMenuItems;
    // }
    ...mapGetters([
      // ใช้ตัวนี้แทน getters ทั้งหมด
      "getMenuItems"
    ]),
    total(){
      var totalCost = 0
      for(var items in this.basket){
        var individualItems = this.basket[items]
        totalCost += individualItems.price*individualItems.quantity
      }
      return totalCost
    }
  },
  methods: {
    addToBasket(item, option) {
      this.basket.push({
        name: item.name,
        price: option.price,
        size: option.size,
        quantity: 1
      });
    },
    removeFromBasket(item) {
      this.basket.splice(this.basket.indexOf(item), 1);
    },
    increaseQuantity(item) {
      item.quantity++;
    },
    decreaseQuantity(item) {
      item.quantity--;
      if (item.quantity == 0) {
        this.removeFromBasket(item);
      }
    },
    addNewOrder() {
      // this.$store.commit("addOrder", this.basket); //เรียก commit mutation ที่ชื่อ addOrder แล้วส่ง this.basket ไป
      dbOrdersRef.push(this.basket);
      this.basket = [];
      this.basketText = "Thank you, your order has been placed :D";
    }
  }
};
</script>   
