<template>
  <div>
    <div class="col-sm-6 col-md-4 col-lg-3">
      <div class="panel panel-success">
        <div class="panel-heading">
          <h3 class="panel-title">{{ stock.name }}
          <small>(Price: {{ stock.price }} | Quantity: {{ stock.quantity }})</small>
          </h3>
        </div>
        <div class="panel-body">
          <div class="pull-left">
            <input type="number" class="form-control" placeholder="Quantity" v-model.number="quantity" />
          </div>
          <div class="pull-right">
            <div class="btn btn-success" :class="{'btn-danger': notEnoughQuantity}" @click="sellStock" :disabled="notEnoughQuantity || quantity <= 0 || !Number.isInteger(quantity)">
              {{ notEnoughQuantity ? 'Not Enough Quantity' : 'SELL' }}
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import {mapActions} from 'vuex';
export default {
  props: ['stock'],
  data() {
    return {
      quantity: 0
    };
  },
  computed: {
    notEnoughQuantity() {
      return this.quantity > this.stock.quantity; 
    }
  },
  methods: {
    ...mapActions({
      placeSellOrder: 'sellStock'
    }),
    sellStock() {
      const order = {
        stockId: this.stock.id,
        stockPrice: this.stock.price,
        quantity: this.quantity
      };
      this.placeSellOrder(order);
      this.quantity = 0;
    }
  },
};
</script>

<style scoped>
</style>
