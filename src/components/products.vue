<template>
  <div>
    <h2>Product List</h2>
    <div class="container py-3">
      <div class="row g-3">
       <div class="col-md-6" v-for="product in filteredProducts" :key="product.productid">
          <div class="list-group">
            <a href="#" class="list-group-item list-group-item-action active" aria-current="true">
              <div class="d-flex w-100 justify-content-between">
                <h5 class="mb-1"><i :class="['fa-solid', product.icon, 'me-2']"></i>{{ product.pname }}</h5>
              </div>
              <p class="mb-1">LOCATION : {{ product.location }}</p>
              <p class="mb-1">AVAILABILITY : {{ product.quantity }}</p>
              <p>PRICE : £{{ product.price }}</p>
              <button type="button" class="btn btn-primary" @click="ADDBasket(product)" :disabled="product.quantity <= 0">ADD BASKET</button>
            </a>
          </div>
        </div>
      </div>
    </div>

    <button class="btn btn-primary" type="button" data-bs-toggle="offcanvas" data-bs-target="#offcanvasScrolling" aria-controls="offcanvasScrolling" :disabled="Basket.length <= 0">Basket</button>

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
                <h5 class="mb-1"><i :class="['fa-solid', product.icon, 'me-2']"></i>{{ product.pname }}</h5>
              </div>
              <p class="mb-1">LOCATION : {{ product.location }}</p>
              <p class="mb-1">AVAILABILITY : {{ product.quantity }}</p>
              <p>PRICE : £{{ product.price }}</p>
              <button type="button" class="btn btn-primary" @click="REMOVEBasket(product)" >REMOVE</button>
            </a>
          </div>
        </div>

      </div>
      <div>
        <h5>Enter your name:</h5>
        <input type="text" v-model="CHECKOUT.NAME" @input="validateName"  placeholder="Enter Name" />
        <h5>Enter your Phone number:</h5>
        <input type="text" v-model="CHECKOUT.PHONE" @input="validatePhone" placeholder="Enter Phone Number" />
        <button class="btn btn-success w-100" @click="CHECK" :disabled="!CHECKOUT.NAME || !CHECKOUT.PHONE">Checkout</button>
      </div>
    </div>

    <div class="d-flex flex-column align-items-start mb-3">
      <div class="filter-box fixed-left">
        <label for="locationFilter" class="form-label me-2 mb-0 fw-bold">Location:</label>
        <select id="locationFilter" class="form-select d-inline-block w-auto border-0 bg-transparent" v-model="selectedLocation">
          <option value="">All Locations</option>
          <option v-for="loc in uniqueLocations" :key="loc" :value="loc">{{ loc }}</option>
        </select>
      </div>
    </div>
    <div class="filter-box fixed-left2" >
      <label for="subject" class="form-label me-2 mb-0 fw-bold">subjsect:</label>
        <select id="subject" class="form-select d-inline-block w-auto border-0 bg-transparent" v-model="selectedsubject">
          <option value="">All subjects</option>
          <option v-for="sub in  selcsubject" :key="sub" :value="sub">{{ sub }}</option>
        </select>
    </div>
    <div class="filter-box fixed-left3">
      <label for="sortPrice" class="form-label me-2 mb-0 fw-bold">Price:</label>
      <select id="sortPrice" class="form-select d-inline-block w-auto border-0 bg-transparent" v-model="selectedprice">
        <option value="">All price</option>
        <option value="low">Low → High</option>
        <option value="high">High → Low</option>
      </select>
    </div>
    <div class="filter-box fixed-left4">
      <label for="sortquantity" class="form-label me-2 mb-0 fw-bold">AVAILABILITY:</label>
      <select id="sortquanity" class="form-select d-inline-block w-auto border-0 bg-transparent" v-model="selectedavaliablity">
        <option value="">All Availability</option>
        <option value="low">Low → High</option>
        <option value="high">High → Low</option>
      </select>
    </div>
  </div>
</template>



<script>
  export default {
    name: 'ProductList', 
    data() {
      return {
        selectedLocation: '',
        selectedsubject: '',
        selectedprice: '',
        selectedavaliablity: '',
        products: [],
        Basket: [],
        CHECKOUT: {NAME: '', PHONE:'' }

      }
    },

    Display() {
      fetch('http://localhost:5000/lessons')
      .then(res => res.json())
      .then(data => {
      this.products = data;
      console.log("Products loaded from backend:", data);
    })
    .catch(err => {
      console.error("Error fetching products:", err);
    });
   },

    computed: {
    uniqueLocations() {
      return [...new Set(this.products.map(p => p.location))];
    },

    selcsubject() {
      return [...new Set(this.products.map(p => p.pname))];
    },

    filteredProducts() {
      let filtered = this.products;

      if (this.selectedLocation) {
        filtered = filtered.filter(p => p.location === this.selectedLocation);
      }

      if (this.selectedsubject) {
        filtered = filtered.filter(p => p.pname === this.selectedsubject);
      }

      if (this.selectedprice === 'low') {
        filtered = [...filtered].sort((a, b) => a.price - b.price);
      } else if (this.selectedprice === 'high') {
        filtered = [...filtered].sort((a, b) => b.price - a.price);
      }

      if (this.selectedavaliablity === 'low') {
        filtered = [...filtered].sort((a, b) => a.quantity - b.quantity);
      } else if (this.selectedprice === 'high') {
        filtered = [...filtered].sort((a, b) => b.quantity - a.quantity);
      }



      return filtered;
    }

   },

    methods: {
      ADDBasket(product){
         if (!product || product.quantity <= 0) return;
          product.quantity -= 1;
          console.log("Removes quantity:", product.quantity);

        this.Basket.push({productid: product.productid, pname: product.pname, location: product.location, price: product.price, quantity: 1, icon: product.icon});
        console.log("Added to basket:", product);
      },

      REMOVEBasket(product) {
      // find the index of the product in the basket
       const index = this.Basket.indexOf(product);

        if (index !== -1) {
           // restore product quantity
          const found = this.products.find(p => p.productid === product.productid);
          if (found) {
           found.quantity += 1;
           console.log("Restored quantity:", found.quantity);
          } 

          // remove the product from the basket
          this.Basket.splice(index, 1);
          console.log("Removed from basket:", product);
        }
      },

      async CHECK() {
        if (!this.CHECKOUT.NAME || !this.CHECKOUT.PHONE) {
          alert("Please enter your name and phone number before checking out.");
         return;
        }

        const orderData = {
          Name: this.CHECKOUT.NAME,
          phone: this.CHECKOUT.PHONE,
          productid: this.Basket.map(p => p.productid),
          pname: this.Basket.map(p => p.pname),
          quantity: this.Basket.map(p => p.quantity)
        };

        try {
          const response = await fetch("http://localhost:5000/Order",{
            method: "POST",
            headers: {"Content-Type": "application/json"},
            body: JSON.stringify(orderData),
          })

          const data = await response.json();

          if(response.ok){
            console.log("Order success:", data);
            alert(`Thank you, ${this.CHECKOUT.NAME}! Your order has been placed.`);

            // Clear basket only after success
           this.Basket = [];
           this.CHECKOUT = { NAME: "", PHONE: "" };
          } else{
            console.error("Order error:", data.error);
            alert(data.error || "Failed to place order. Please try again.");
          }
        } catch(error){
          console.error(" Unexpected error:", error);
          alert("An unexpected error occurred. Please try again later.");
        }
      }  

    }

  }
  










</script>

<style scoped>
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

.filter-box {
  display: inline-block;
  padding: 8px 16px;
  border-radius: 30px;     
  border: 2px solid #0d6efd; 
  background-color: #f8f9fa; 
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); 
}


.fixed-left {
  position: fixed;
  top: 150px;    
  left: 10px;   
}


.fixed-left2 {
  position: fixed;
  top: 225px;    
  left: 10px;    
}

.fixed-left3 {
  position: fixed;
  top: 300px;    
  left: 10px;    
}

.fixed-left4 {
  position: fixed;
  top: 375px;    
  left: 10px;    
}

</style>
  