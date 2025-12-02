<template>
  <div class="main-wrapper">

    <div v-if="!showBasket">

      <!--contains the Search bar and basket button-->
      <div class="top-bar d-flex justify-content-between align-items-center px-4">
        <h1 class="m-0 fw-normal">hilessons</h1>
        <div class="search-container mx-auto">
          <input type="text" class="form-control search-input" placeholder="Search lessons..." v-model="searchQuery" @input="searchproduct">  <!--search input which is linked to searchQuery-->
        </div>
        <button class="btn btn-primary" @click="showBasket = true" :disabled="Basket.length <= 0"> Basket ({{ Basket.length }})</button>  <!--button that opens the basket page-->
      </div>

      <div class="filters-left">

        <!--list of diffrent sorting methods-->
        <div class="filter-box fixed-left">
          <label for="locationFilter" class="form-label me-2 mb-0 fw-bold">Location:</label>
          <select id="locationFilter" class="form-select d-inline-block w-auto border-0 bg-transparent" v-model="selectedLocation">  <!--loaction linked to our selectedlocation-->
            <option value="">All Locations</option>
            <option v-for="loc in uniqueLocations" :key="loc" :value="loc">{{ loc }}</option>
          </select>
        </div>

        <div class="filter-box fixed-left2">
          <label for="subject" class="form-label me-2 mb-0 fw-bold">subject:</label>
          <select id="subject" class="form-select d-inline-block w-auto border-0 bg-transparent" v-model="selectedsubject"> <!--subject linked to our selectedsubject-->
            <option value="">All subjects</option>
            <option v-for="sub in selcsubject" :key="sub" :value="sub">{{ sub }}</option>
          </select>
        </div>

        <div class="filter-box fixed-left3">
          <label class="form-label me-2 mb-0 fw-bold">Sort Subject A-Z:</label>
          <select class="form-select d-inline-block w-auto border-0 bg-transparent" v-model="subjectSort" > <!--sort linked to our selectedsort-->
            <option value="">None</option>
            <option value="A">A → Z</option>
            <option value="Z">Z → A</option>
          </select>
        </div>

        <div class="filter-box fixed-left4">
          <label for="sortPrice" class="form-label me-2 mb-0 fw-bold">Price:</label>
          <select id="sortPrice" class="form-select d-inline-block w-auto border-0 bg-transparent" v-model="selectedprice"> <!--price linked to our selectedprice-->
            <option value="">All price</option>
            <option value="low">Low → High</option>
            <option value="high">High → Low</option>
          </select>
        </div>

        <div class="filter-box fixed-left5">
          <label for="sortquantity" class="form-label me-2 mb-0 fw-bold">AVAILABILITY:</label>
          <select id="sortquantity" class="form-select d-inline-block w-auto border-0 bg-transparent" v-model="selectedavaliablity"> <!-- avalibilty linked to our selectedavaliability-->
            <option value="">All Availability</option>
            <option value="low">Low → High</option>
            <option value="high">High → Low</option>
          </select>
        </div>

      </div>

      <!--where products are displayed and added -->
      <div class="products-area">
        <div class="container py-3">
          <div class="row g-3">
            <div class="col-md-6" v-for="product in filteredProducts" :key="product.productid"> <!-- loops through filtered products and key is for finding the products -->
              <div class="list-group">
                <a href="#" class="list-group-item list-group-item-action active" aria-current="true">
                  <div class="d-flex w-100 justify-content-between">
                    <h5 class="mb-1">
                      <i :class="['fa-solid', product.icon, 'me-2']"></i>{{ product.pname }} <!-- uses fontaswesome icons and shows product name-->
                    </h5>
                  </div>
                  <p class="mb-1">LOCATION : {{ product.location }}</p> <!-- shows location -->
                  <p class="mb-1">AVAILABILITY : {{ product.quantity }}</p> <!-- shows quantity -->
                  <p>PRICE : £{{ product.price }}</p> <!--shows the price-->
                  <button type="button" class="btn btn-primary" @click="ADDBasket(product)" :disabled="product.quantity <= 0"> ADD BASKET</button> <!--calls the addBasket(product) method and if quantity is 0 button is disabled-->
                </a>
              </div>
            </div>
          </div>
        </div>
      </div> 

      <div class="footer-bar"></div>

    </div>

    <!-- basket page -->
    <div v-if="showBasket" class="basket-show"> <!-- changes the page layout when show basket is true -->
      
      <div class="basket-header">
        <h2 class="basket-title">BASKET ({{ Basket.length }})</h2> <!-- basket title and shows the amount of items in the basket -->
        <button class="btn btn-secondary" @click="showBasket = false"><i class="fa-solid fa-arrow-left"></i>Back</button> <!-- makes the basket page false and renders in the main page -->
      </div>
      
      <div class="basket-layout">
        <div class="basket-order-box">
          <h3>Your order:</h3>
          <div class="basket-order-scroll"> <!-- Allows the order box to be scrolled through  -->
            <div class="list-group-item list-group-item-action active" v-for="product in Basket" :key="product.productid"> <!-- loops through the basket and key is for finding the products -->
              <h5><i :class="['fa-solid', product.icon, 'me-2']"></i>{{ product.pname }}</h5> <!-- uses fontaswesome icons and shows product name-->
              <p>LOCATION : {{ product.location }}</p>  <!-- shows location -->
              <p>AVAILABILITY : {{ product.quantity }}</p> <!-- shows quantity -->
              <p>PRICE : £{{ product.price * product.quantity }}</p>  <!--shows the toatl price of that product depenig of how many lessons u have boghut of that type-->
              <button class="btn btn-danger w-100" @click="REMOVEBasket(product)">REMOVE</button> <!--calls the removeBasket(product) method and removes the item one at a time-->
            </div>
          </div>
        </div>
        
        <div class="basket-checkout-box">
          <h3>Total Price</h3> 
          <h2>£{{ Basket.reduce((sum, p) => sum + p.price * p.quantity, 0) }}</h2><!-- Shows the total price-->
          
          <label class="mt-3">Enter Name</label>
          <input class="form-control" v-model="CHECKOUT.NAME"> <!--binds checkout name -->
          
          <label class="mt-3">Phone Number</label>
          <input class="form-control" v-model="CHECKOUT.PHONE"> <!-- binds checkout phone -->
          <button class="btn btn-success w-100 mt-4" @click="CHECK" :disabled="!CHECKOUT.NAME || !CHECKOUT.PHONE">CHECKOUT</button> <!-- calls the check method and is disabled if there is no name and phone number -->
        </div>
      </div>
      <div class="footer-bar"></div>
    </div>

  </div>
</template>




<script>
  export default {
    name: 'ProductList', 
    data() {
      return {
        showBasket: false,
        searchQuery: "", // stores text in the search bar
        selectedLocation: '', // selected location filter
        selectedsubject: '', // selected subject filter
        selectedprice: '', // selected price filter
        subjectSort: "", // sorts subject A to Z and Z to A 
        selectedavaliablity: '', // sorts by avaliablity
        products: [], // stores all the products loaded from backend 
        Basket: [], // holds items that have been added in the basket 
        CHECKOUT: {NAME: '', PHONE:'' } // checkout for input values

      }
    },

    mounted() {
      // fetch products from backend when the page is loaded
      fetch('https://back-end-1-2uy7.onrender.com/lessons')
      .then(res => res.json()) // convert response to JSON
      .then(data => {
      this.products = data;  // store the products
      console.log("Products loaded from backend:", data);
    })
    .catch(err => {
      console.error("Error fetching products:", err); // error handling
    });
   },

    computed: {
    //create a list of location of the product location for the dropdown menu
    uniqueLocations() {
      return [...new Set(this.products.map(p => p.location))];
    },
    // creates a list of subjects for the dropdrop down mwnu 
    selcsubject() {
      return [...new Set(this.products.map(p => p.pname))];
    },

    // appls all fillters to the products 
    filteredProducts() {
      let filtered = this.products;
      
      // fillter by location
      if (this.selectedLocation) {
        filtered = filtered.filter(p => p.location === this.selectedLocation);
      }
      
      // fillters by subject
      if (this.selectedsubject) {
        filtered = filtered.filter(p => p.pname === this.selectedsubject);
      }

      // fillters by price(low to high)
      if (this.selectedprice === 'low') {
        filtered = [...filtered].sort((a, b) => a.price - b.price);
        // fillters by price(high to low)
      } else if (this.selectedprice === 'high') {
        filtered = [...filtered].sort((a, b) => b.price - a.price);
      }

      // fillters by avaliablity(low to high)
      if (this.selectedavaliablity === 'low') {
        filtered = [...filtered].sort((a, b) => a.quantity - b.quantity);
      } else if (this.selectedavaliablity === 'high') {
        // fillters by avaliablity(high to low)
        filtered = [...filtered].sort((a, b) => b.quantity - a.quantity);
      }

      // sorts subject alphabetically a to z 
      if (this.subjectSort === "A") {
        filtered = [...filtered].sort((a, b) => a.pname.localeCompare(b.pname));
      } else if (this.subjectSort === "Z") {
        // fsorts subjects alphabetically z to a 
         filtered = [...filtered].sort((a, b) => b.pname.localeCompare(a.pname));
      }

      return filtered; //return final filltered list
    }

   },

    methods: {

      // adds product to basket 
      async ADDBasket(product){
         if (!product || product.quantity <= 0) return; //prevent invaild actions 
          product.quantity -= 1; //reduces available stock and changes quantity value on the ui 
          console.log("Removes quantity:", product.quantity);

          //checks if item already exists in basket
          const existing = this.Basket.find(item => item.productid === product.productid);

          if (existing) {
            existing.quantity += 1;// increases quantity if already added 
          } else {this.Basket.push({ productid: product.productid,pname: product.pname,location: product.location,price: product.price, quantity: 1,icon: product.icon}); // adds new product to basket 
            console.log("Added to basket:", product);
          }
      },

      //remove a product from basket 
      async REMOVEBasket(product) {

      // find the index of the product in the basket
       const index = this.Basket.indexOf(product);

        if (index !== -1) {
          const basketItem = this.Basket[index];

          // restore product quantity
          const found = this.products.find(p => p.productid === product.productid);
          if (found) {
           found.quantity += 1;
           console.log("Restored quantity:", found.quantity);
          } 
          
          // if more than 1 in the basket it reduces it by 1
          if (basketItem.quantity > 1) {
            basketItem.quantity -= 1;
          } else {
            // removes item from the basket 
            this.Basket.splice(index, 1);
            console.log("Removed from basket:", product);
          }
          
        }
      },

      // checkout and send order to backend
      async CHECK() {
        if (!this.CHECKOUT.NAME || !this.CHECKOUT.PHONE) {
          alert("Please enter your name and phone number before checking out.");
         return;
        }

        // calculate total price
        const totalPrice = this.Basket.reduce((sum, p) => sum + p.price * p.quantity, 0);

        // Prepare order data structure
        const orderData = {
          Name: this.CHECKOUT.NAME,
          phone: this.CHECKOUT.PHONE,
          productid: this.Basket.map(p => p.productid),
          pname: this.Basket.map(p => p.pname),
          quantity: this.Basket.map(p => p.quantity),
          total: totalPrice
        };

        try {
          //sends order to backend 
          const response = await fetch("https://back-end-1-2uy7.onrender.com/Order",{
            method: "POST",
            headers: {"Content-Type": "application/json"},
            body: JSON.stringify(orderData),
          })

          const data = await response.json();

          if(response.ok){
            console.log("Order success:", data);

             // update backend stock for each product in order
            for (const item of this.Basket) {
              const product = this.products.find(p => p.productid === item.productid);

              if (product) {
                const newQty = product.quantity;
                await fetch("https://back-end-1-2uy7.onrender.com/quantity", {
                  method: "PUT",
                  headers: { "Content-Type": "application/json" },
                  body: JSON.stringify({
                    pname: product.pname,
                    quantity: newQty
                  })
                });
                // Update UI
               product.quantity = newQty;
              }
            }

            alert(`Thank you, ${this.CHECKOUT.NAME}! Your order has been placed.`);

            // clear basket only after success
           this.Basket = [];
           this.CHECKOUT = { NAME: "", PHONE: "" };
           this.showBasket = false;
          } else{
            console.error("Order error:", data.error);
            alert(data.error || "Failed to place order. Please try again.");
          }
        } catch(error){
          console.error(" Unexpected error:", error);
          alert("An unexpected error occurred. Please try again later.");
        }
      },
      
      // search method
      async searchproduct(){

        //get the value from search input
        const query = this.searchQuery.trim();

        // If search is empty reloads all products
        if (!query) {
          const res = await fetch("https://back-end-1-2uy7.onrender.com/lessons");
          this.products = await res.json();
          return;
        }
        
        try{
          //sends a request to backend search route
          const response = await fetch (`https://back-end-1-2uy7.onrender.com/lessons/search?query=${encodeURIComponent(this.searchQuery)}`);
          const data = await response.json();
          
          if (response.ok) {
            this.products = data.products; // update ui with the results 
          }else{
            console.error("Search error", data.message);
          }
        } catch (error) {
          console.error("Unexpected search error", error);
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

.fixed-left5 {
  position: fixed;
  top: 450px;
  left: 10px;
}


.top-bar {
  position: fixed;
  top: 0;
  width: 100%;
  height: 90px;
  border-bottom: 2px solid black;
  background: white;
  z-index: 2000;
}

.basket-btn {
  border-width: 2px !important;
  border-radius: 25px;
  padding: 10px 25px;
}


.filters-left {
  position: fixed;
  top: 110px;           
  left: 20px;
  width: 260px;
  display: flex;
  flex-direction: column;
  gap: 25px;
  padding-bottom: 100px; 
}


.products-area {
  margin-top: 110px;   
  margin-bottom: 90px; 
  margin-left: 330px;  
  padding-right: 20px;
  padding-bottom: 200px
}


.footer-bar {
  position: fixed;
  bottom: 0;
  width: 100%;
  height: 70px;
  background: rgb(102, 142, 250);
  border-top: 2px solid black;
  z-index: 2000;
}

.offcanvas {
  z-index: 5000 !important;
}

.search-container {
  width: 40%;
}

.search-input {
  border: 2px solid #0d6efd;
  border-radius: 25px;
  padding: 8px 15px;
}

.basket-show {
  width: 100%;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  background: white;
}


.basket-header {
  position: relative;
  width: 100%;
  text-align: center;
  margin-top: 30px;
  padding-bottom: 10px;
  border-bottom: 3px solid black;
}

.basket-title {
  font-size: 34px;
  font-weight: bold;
}


.back-btn {
  position: absolute;
  left: 20px;
  top: -5px;
  display: flex;
  align-items: right;
  gap: 6px;
}


.basket-layout {
  display: flex;
  justify-content: center;
  gap: 40px;
  margin-top: 40px !important;  
  flex-grow: 1;
}


.basket-order-box {
  width: 25%;
  height: 63vh;                  
  background: white;
  border: 3px solid black;
  border-radius: 40px;
  padding: 25px 35px;
  display: flex;
  flex-direction: column;
}


.basket-order-scroll {
  overflow-y: auto;
  margin-top: 10px;
  padding-right: 10px;
  height: calc(100% - 40px);
}


.basket-order-box .list-group-item.active {
  background: #d3d3d3;
  border-radius: 30px;
  border: 2px solid black !important;
  padding: 20px;
  margin-bottom: 20px;
}


.basket-checkout-box {
  width: 25%;
  height: 63vh;
  background: white;
  border: 3px solid black;
  border-radius: 40px;
  padding: 30px 30px;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
}




</style>
  