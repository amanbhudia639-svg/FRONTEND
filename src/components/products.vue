<template>
  <div>
    <h2>Product List</h2>
    <div class="container py-3">
      <div class="row g-3">
        <div class="col-md-6" v-for="product in products" :key="product.productid">
          <div class="list-group">
            <a href="#" class="list-group-item list-group-item-action active"  aria-current="true">
              <div class="d-flex w-100 justify-content-between">
                <h5 class="mb-1">{{ product.pname }}</h5>
              </div>

              <p class="mb-1">LOCATION : {{ product.location }}</p>
              <p class="mb-1">AVAILABILITY : {{ product.showavaliable }}</p>
              <p>PRICE : {{ product.price }}</p>

              <button type="button" class="btn btn-primary">
                ADD BASKET
              </button>
            </a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
  export default {
    name: 'ProductList', // multi-word to satisfy ESLint
    data() {
      return {
        products: [
          { productid: '1001', pname: 'Maths',      location: 'Harrow',   price: '£10', showavaliable: '10' },
          { productid: '1002', pname: 'Science',    location: 'Stanmore', price: '£15', showavaliable: '8'},
          { productid: '1003', pname: 'Geography',  location: 'Harrow',   price: '£6',  showavaliable: '7'},
          { productid: '1004', pname: 'History',    location: 'Harrow',   price: '£10', showavaliable: '15'},
          { productid: '1005', pname: 'English',    location: 'Harrow',   price: '£10', showavaliable: '2'}
        ]
      }
    }
  }
</script>

<style >
   h2 { margin-bottom: 16px; }

   ul {
    list-style: none;
    padding: 0;
    margin: 0;
    display: grid;
    grid-template-columns: repeat(2,1fr); 
    gap: 20px;
  }
  
  li {
   margin-bottom: 10px; 
   padding: 10px;
   border-radius: 6px;
  }
  
  .list-group-item.active {
   background-color: grey; 
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
  