<template>
  <div class="col product">
    <div class="prd-image"
         :style="{background: `url(${product.Image}) center center no-repeat`}"
         @click="goToDetails(product.Title)"></div>
    <h3 class="product-title" :title="product.Title">{{ productTitle }}</h3>
    <div class="product-price">
      <div class="row">
        <div class="col">
          <strong>{{ $store.state.currency }}</strong> {{ product.Price }}
        </div>
        <div class="col">
          <input type="number" class="inline-el form-control qty-field" v-model="qty" min="1"/>
          <span class="inline-el">Qty.</span>
        </div>
      </div>
    </div>
    <span class="add-to-cart btn btn-primary btn-lg btn-block" @click="addToCart">Add To Cart</span>
  </div>
</template>

<script>
export default {
  name: "Product",
  data() {
    return {
      titleLen: 40,
      qty: 1
    }
  },
  props: {
    product: {
      type: Object,
      default: () => {
      }
    }
  },
  computed: {
    price() {
      return `<strong>${this.$store.state.currency}</strong> ${this.product.Price}`;
    },
    productTitle() {
      if (this.product.Title.length > this.titleLen) {
        return `${this.product.Title.substr(0, this.titleLen)}...`
      }
      return this.product.Title
    }
  },
  methods: {
    loadLargeImage(product) {
      this.$store.commit('selectProduct', product);
    },
    goToDetails(productName) {
      this.$router.push(`/details/${productName}`)
    },
    addToCart() {
      // const clonedProduct = JSON.parse(JSON.stringify(this.product));
      const clonedProduct = Object.assign({}, this.product);
      clonedProduct.quantity = this.qty;
      this.$store.commit('addToCart', clonedProduct);
      console.log(this.$store.getters.getProducts);
    }
  }
}
</script>

<style scoped>
div.product {
  padding: 4px;
  margin-bottom: 10px;
}

div.prd-image {
  width: 94%;
  height: 300px;
  background-size: auto 100% !important;
  cursor: pointer;
}

h3.product-title {
  font-size: 18px;
  padding: 6px 0 6px 0;
  text-transform: uppercase;
  font-weight: 300;
  text-align: center;
  min-height: 90px;
}

div.product-price {
  text-align: center;
  margin-bottom: 10px;
}

.add-to-cart {
  margin-bottom: 14px;
}

.inline-el {
  display: block;
  float: right;
 
}


.qty-field {
  width: 70px;
  margin: 0 50px 0 10px;
}
</style>