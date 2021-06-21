<template>
<div class="container">
  <main>
    <div class="py-5 text-center">
      <h2>Welcome</h2>
      <p class="lead">{{ user.first_name }} {{ user.last_name }} has invited you to buy these products!</p>
    </div>

    <div class="row g-5">
      <div class="col-md-5 col-lg-4 order-md-last">
        <h4 class="d-flex justify-content-between align-items-center mb-3">
          <span class="text-primary">Products</span>
        </h4>
        <ul class="list-group mb-3">
          <template v-for="product in products">
            <li class="list-group-item d-flex justify-content-between lh-sm">
              <div>
                <h6 class="my-0">{{ product.title }}</h6>
                <small class="text-muted">{{ product.description }}</small>
              </div>
              <span class="text-muted">{{ product.price }}</span>
            </li>
            <li class="list-group-item d-flex justify-content-between lh-sm">
              <div>
                <h6 class="my-0">Quantity</h6>
              </div>
              <input v-model="quantities[product.id]" class="text-muted form-control quantity" type="number" min="0">
            </li>
          </template>
          <li class="list-group-item d-flex justify-content-between">
            <span>Total (USD)</span>
            <strong>${{ total }}</strong>
          </li>
        </ul>
      </div>

      <div class="col-md-7 col-lg-8">
        <h4 class="mb-3">Personal Info</h4>
        <form class="needs-validation" novalidate>
          <div class="row g-3">
            <div class="col-sm-6">
              <label for="firstName" class="form-label">First name</label>
              <input type="text" class="form-control" id="firstName" placeholder="Fist Name" required>
            </div>

            <div class="col-sm-6">
              <label for="lastName" class="form-label">Last name</label>
              <input type="text" class="form-control" id="lastName" placeholder="" value="Last Name" required>
            </div>

            <div class="col-12">
              <label for="email" class="form-label">Email <span class="text-muted">(Optional)</span></label>
              <input type="email" class="form-control" id="email" placeholder="you@example.com">
            </div>

            <div class="col-12">
              <label for="address" class="form-label">Address</label>
              <input type="text" class="form-control" id="address" placeholder="1234 Main St" required>
            </div>

            <div class="col-md-5">
              <label for="country" class="form-label">Country</label>
              <input type="text" class="form-control" id="country" placeholder="Country">
            </div>

            <div class="col-md-4">
              <label for="city" class="form-label">City</label>
              <input type="text" class="form-control" id="city" placeholder="City">
            </div>

            <div class="col-md-3">
              <label for="zip" class="form-label">Zip</label>
              <input type="text" class="form-control" id="zip" placeholder="Zip">
            </div>
          </div>

          <hr class="my-4">

          <button class="w-100 btn btn-primary btn-lg" type="submit">Checkout</button>
        </form>
      </div>
    </div>
  </main>
</div>
</template>

<script lang="ts">
import Vue from 'vue'
import {Context} from "@nuxt/types";

export default Vue.extend({
  async asyncData(ctx: Context) {
    const {data} = await ctx.$axios.get(`links/${ctx.params.code}`);

    const user = data.user;
    const products = data.products;
    let quantities = [];

    products.forEach((p: any) => {
      quantities[p.id] = 0;
    });

    return {
      user,
      products,
      quantities
    }
  },
  data() {
    return {
      user: null,
      products: [],
      quantities: []
    }
  },
  computed: {
    total() {
      return this.products.reduce((s: number, p: any) => {
        return s + p.price * this.quantities[p.id];
      }, 0);
    }
  }
}) 
</script>

<style>
.quantity {
  width: 65px;
}
</style>
