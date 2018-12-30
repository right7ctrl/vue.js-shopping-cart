<template>
  <div id="app">
    <div class="container">
      <div class="row">
        <div class="col-12">
          <div class="table-responsive">
            <table class="table table-striped">
              <thead>
              <tr>
                <th scope="col">Items({{itemCount}})</th>
                <th scope="col">Product</th>
                <th scope="col" class="text-center">Quantity</th>
                <th scope="col" class="text-right">Price</th>
              </tr>
              </thead>
              <tbody>
              <tr v-for="item in cart[0]">
                <td><img height="50" width="50" v-bind:src="item.image"/></td>
                <td>{{item.name}}</td>
                <td><input class="form-control" v-on:change="updateQuantity" v-bind:id="item.id" type="number" min="1"
                           max="99" value="1"/></td>
                <td class="text-right">{{item.price * item.quantity}} ₺</td>
              </tr>
              <tr>
                <td></td>
                <td></td>
                <td>Sub-Total</td>
                <td class="text-right">{{subTotal}}₺</td>
              </tr>
              <tr v-if="shipping">
                <td></td>
                <td></td>
                <td>Shipping</td>
                <td class="text-right">{{shipping}} ₺</td>
              </tr>
              <tr>
                <td>
                  <button @click="clearCart">Clear Cart</button>
                </td>
                <td></td>
                <td><strong>Total</strong></td>
                <td class="text-right"><strong>{{subTotal + shipping}} ₺</strong></td>
              </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import items from './assets/items.json';

  export default {
    name: 'app',
    data() {
      return {
        subTotal: 0,
        itemCount: 0,
        shipping: 8,
        taxRate: 18,
        cart: []
      }
    },
    methods: {
      updateQuantity(event) {
        var item_id = event.target.id;
        this.cart[0][item_id - 1].quantity = event.target.value;
        this.calcSubTotal(event, item_id);
      },
      calcSubTotal(event, item_id) {
        var i = 0;
        var id = item_id - 1;
        this.subTotal += this.cart[0][id].quantity * this.cart[0][id].price;
      },
      clearCart() {
        this.cart = [];
        this.shipping = 0;
        this.itemCount = 0;
        this.subTotal = 0;
      }
    },
    created() {
      this.cart.push(items);
      if (this.cart.length <= 0) {
        this.shipping = 0;
      }
      this.itemCount = this.cart[0].length;
      var i = 0;
      for (i; i <= this.cart[0].length; i++) {
        this.subTotal += (this.cart[0][i].quantity * this.cart[0][i].price);
      }


    }
  }
</script>

<style lang="scss">
  @import "./../node_modules/bootstrap/scss/bootstrap.scss";

  .bloc_left_price {
    color: #c01508;
    text-align: center;
    font-weight: bold;
    font-size: 150%;
  }

  .category_block li:hover {
    background-color: #007bff;
  }

  .category_block li:hover a {
    color: #ffffff;
  }

  .category_block li a {
    color: #343a40;
  }

  .add_to_cart_block .price {
    color: #c01508;
    text-align: center;
    font-weight: bold;
    font-size: 200%;
    margin-bottom: 0;
  }

  .add_to_cart_block .price_discounted {
    color: #343a40;
    text-align: center;
    text-decoration: line-through;
    font-size: 140%;
  }

  .product_rassurance {
    padding: 10px;
    margin-top: 15px;
    background: #ffffff;
    border: 1px solid #6c757d;
    color: #6c757d;
  }

  .product_rassurance .list-inline {
    margin-bottom: 0;
    text-transform: uppercase;
    text-align: center;
  }

  .product_rassurance .list-inline li:hover {
    color: #343a40;
  }

  .reviews_product .fa-star {
    color: gold;
  }

  .pagination {
    margin-top: 20px;
  }

  footer {
    background: #343a40;
    padding: 40px;
  }

  footer a {
    color: #f8f9fa !important
  }

</style>
