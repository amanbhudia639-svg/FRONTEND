<template>
  <div>
    <h2>Product List</h2>
    <div class="container py-3">
      <div class="row g-3">
        <div class="col-md-6" v-for="product in products" :key="product.productid">
          <div class="list-group">
            <a href="#" class="list-group-item list-group-item-action active" aria-current="true">
              <div class="d-flex w-100 justify-content-between">
                <h5 class="mb-1">{{ product.pname }}</h5>
              </div>
              <p class="mb-1">LOCATION : {{ product.location }}</p>
              <p class="mb-1">AVAILABILITY : {{ product.quantity }}</p>
              <p>PRICE : £{{ product.price }}</p>
              <button type="button" class="btn btn-primary" @click="ADDBasket(product)" :disabled="product.quantity <= 0"> ADD BASKET</button>
            </a>
          </div>
        </div>
      </div>
    </div>

    <button class="btn btn-primary" type="button" data-bs-toggle="offcanvas" data-bs-target="#offcanvasScrolling" aria-controls="offcanvasScrolling" :disabled="Basket <= 0">Basket</button>

    <div class="offcanvas offcanvas-start" data-bs-scroll="true" data-bs-backdrop="false" tabindex="-1" id="offcanvasScrolling" aria-labelledby="offcanvasScrollingLabel">
      <div class="offcanvas-header">
        <h5 class="offcanvas-title" id="offcanvasScrollingLabel">BASKET({{ Basket.length }})</h5>
        <button type="button" class="btn-close" data-bs-dismiss="offcanvas" aria-label="Close"></button>
      </div>

      <div class="offcanvas-body">
        <div class="col-md-6" v-for="product in Basket" :key="product.productid">
          <div class="list-group">
            <a href="#" class="list-group-item list-group-item-action active" aria-current="true">
              <div class="d-flex w-100 justify-content-between">
                <h5 class="mb-1">{{ product.pname }}</h5>
              </div>
              <p class="mb-1">LOCATION : {{ product.location }}</p>
              <p class="mb-1">AVAILABILITY : {{ product.quantity }}</p>
              <p>PRICE : £{{ product.price }}</p>
            </a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>



<script>
  export default {
    name: "ProductList",
   data() {
     return {
       products: [
        { productid: "1001", pname: "Maths", location: "Harrow", price: 10, quantity: 1 },
        { productid: "1002", pname: "Science", location: "Stanmore", price: 15, quantity: 8 },
        { productid: "1003", pname: "Geography", location: "Harrow", price: 6, quantity: 7 },
        { productid: "1004", pname: "History", location: "Harrow", price: 10, quantity: 15 },
        { productid: "1005", pname: "English", location: "Harrow", price: 10, quantity: 2 },
        { productid: "1007", pname: "computer science", location: "Harrow", price: 10, quantity: 10 },
        { productid: "1008", pname: "pre", location: "Stanmore", price: 15, quantity: 8 },
        { productid: "1009", pname: "Art", location: "Harrow", price: 6, quantity: 7 },
        { productid: "10010", pname: "French", location: "Harrow", price: 10, quantity: 15 },
        { productid: "10011", pname: "DT", location: "Harrow", price: 10, quantity: 2 }
        ],
        Basket: [],

      };
   },

   methods: {
     ADDBasket(product) {
       if (!product || product.quantity <= 0) return;

       product.quantity -= 1;
       this.Basket.push({ productid: product.productid, pname: product.pname, location: product.location, price: product.price, quantity: 1});
     },
   }

  };
</script>

<style scoped>
  h2 {
    margin-bottom: 16px;
  }

  ul {
    list-style: none;
    padding: 0;
    margin: 0;
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 20px;
  }

  li {
    margin-bottom: 10px;
    padding: 10px;
    border-radius: 6px;
  }

  .list-group-item.active {
    background-color: lightgrey;
    border-color: black;
    color: black;
    border-width: 2px !important;
    padding: 10px;
  }

  .list-group {
    position: relative;
    top: 100px;
    left: 100px;
  }

</style>

  