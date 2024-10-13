<script setup>
// State
import { ref } from 'vue';

const myData = ref(null);
const usersName = ref('');
const usersEmail = ref('');
const userArray = ref([]);

  function fetchMyData(){
    fetch("https://mahauser2.github.io/productsDummyJson/products.json")
    .then(res => res.json())
    .then(data => {
      myData.value = data;
    })
    .catch(err => console.log(err));
  }

  function submitForm() {
    alert('Du er nu tilmeldt!');

    const newUser = {
      name: usersName.value,
      email: usersEmail.value
    };

    userArray.value.push(newUser);

    usersName.value = '';
    usersEmail.value = '';

    console.log(userArray.value);
  }

  fetchMyData();
</script>


<template>
  <!-- 1: Start out by fetching the products from https://mahauser2.github.io/productsDummyJson/products.json - store the fetched data in state  -->


  <!-- 2: Make the .card (the div further down) dynamic - we need one card for each object in the products array -->

  <!-- 3: Bind the relevant data from the objects in products to the .card in the template/html, you will need both attribute binding and to bind text -->

  <!-- 4: Theres a sale on the category 'electronics'. You need to conditionally render this element JUST below the <div class="card"> IF the product is an electronic product:
    <span class="rotate">On Sale</span>
    -->

  <!-- 5: To further draw attention to the electronics, add the class .featured to the product name (the <h3>), IF its category is electronics (conditional class).
    
      If you managed to implement 4 & 5, your products in the category electronics should look something like this: https://mmd.ucn.dk/lecturer/mfw/assets/img/vue-binding-example2.png  
    -->

  <!-- 6: Some of the items are almost out of stock. You should indicate this to the user so they know to buy the item right away. If count is < 100, add the class "red" to the p.stock, if count is >= 300, add the class "green" -->

  <!--  -->
  <!-- v-model -->
  <!--  -->

  <!-- 7: We need to setup our newsletter, bind the input fields to state -->

  <!-- 8: When the user has entered a username and an email (he has submitted the form), alert the user something like this: https://mmd.ucn.dk/lecturer/mfw/assets/img/vue-binding-example1.png also add the user to the users array in reactive data -->

  <h1 class="main-title">A great webshop</h1>
  <div class="newsletter center">
    <p>Sign up for our newsletter here!</p>
    <form @submit.prevent="submitForm">
      <div>
        <label for="username">Username:</label>
        <input type="text" id="username" name="username" placeholder="Username:" v-model="usersName" required />
      </div>
      <div>
        <label for="email">email:</label>
        <input type="email" id="email" name="email" placeholder="Email:" v-model="usersEmail" required />
      </div>
      <button type="submit">Sign Up</button>
    </form>
  </div>
  <div class="product-wrapper center">
    <div v-for="product in myData" :key="myData.id" class="card">
      <span v-if="product.category == 'electronics'" class="rotate">On Sale</span>
      <div class="product">
        <div class="product-image">
          <img :src="product.image" :alt="product.alt" />
        </div>
        <div class="product-info">
          <header>
            <h3 :class="{ featured: product.category === 'electronics' }">Taske</h3>
            <p class="price">{{ product.price }} kr</p>
            <p class="category">{{ product.category }}</p>
          </header>
          <p class="description">{{product.description}}</p>
          <p :class="{ red: product.rating.count < 100 }, { green: product.rating.count > 300 }" class="stock">In stock: {{ product.rating.count }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.center {
  max-width: 1200px;
  margin: 0 auto;
}

.main-title {
  text-align: center;
  color: #eee;
  padding: 2rem;
  border-bottom: 2px solid #eee;
}

.product-wrapper {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
}

@media screen and (min-width: 1000px) {
  .product-wrapper {
    grid-template-columns: repeat(3, 1fr);
  }
}
.card {
  display: flex;
  flex-direction: column;
  max-width: 400px;
  background: white;
  padding: 1rem 2rem;
  box-shadow: var(--stdBoxshadow);
  margin: 5px;
  position: relative;
}

.product-image img {
  max-width: 60%;
  margin: 10px 20%;
}

header {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}
header h3,
header .category {
  width: 50%;
}
header > * {
  margin-top: 20px;
}
header h3 {
  font-size: 14px;
  line-height: 130%;
}
header .category {
  font-weight: bold;
  color: #6d6d6d;
  margin-top: 5px;
}
header .price {
  color: var(--orangeBg);
  font-size: 18px;
  font-weight: bold;
}

.description {
  font-size: 12px;
}

.newsletter {
  padding: 1rem;
}
.newsletter > * {
  margin: 5px;
}
.newsletter p {
  color: var(--complementaryColor);
  font-size: 18px;
  font-weight: bold;
  margin: 1rem 0;
}
.newsletter form {
  display: flex;
  flex-direction: column;
  max-width: 400px;
}
.newsletter form > div {
  display: flex;
  justify-content: space-between;
  max-width: 400px;
  margin: 5px;
}
.newsletter input {
  padding: 5px 10px;
  border: none;
  border-radius: 5px;
  box-shadow: var(--stdBoxshadow);
}
.newsletter button {
  color: #eee;
  margin-top: 0.5rem;
  padding: 0.7rem 1.4rem;
  border-radius: 6px;
  background: #333;
  border: none;
  box-shadow: var(--stdBoxshadow);
  text-align: center;
}

.featured {
  color: var(--complementaryColor);
  font-size: 16px;
  font-weight: bold;
}

.rotate {
  transform: rotate(90deg);
  max-width: 100px;
  transform-origin: top left;
  color: white;
  background: var(--complementaryColor);
  padding: 0.3rem;
  position: absolute;
  top: 0px;
  left: 37px;
  box-shadow: 3px 0px 5px 0px rgba(0, 0, 0, 0.5);
}

.green {
  color: #1b9a1b;
}

.red {
  color: #fb4343;
  font-weight: bold;
}
</style>
