<script setup>
import { ref, onMounted, watch } from "vue";
import { db } from "./data/guitars";
import Guitar from "./components/Guitar.vue";
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";

const guitars = ref([]);
const cart = ref([]);
const promGuitar = ref([])

watch( cart, () => {
  saveLocalStorage() //-> esccha todo los cambios en el componente, en este caso en el carrito de compras
}, {
  deep: true
})

onMounted(() => {
  guitars.value = db;
  promGuitar.value = db[3]

  const cartStorage = localStorage.getItem('cart')
  if (cartStorage) {
    cart.value = JSON.parse(cartStorage)
  }
});

const saveLocalStorage = () => {
  localStorage.setItem('cart', JSON.stringify(cart.value))
}

const addToCart = (guitar) => {
  const existCart = cart.value.findIndex(product => product.id === guitar.id)
  if (existCart >= 0) {
    cart.value[existCart].quantity++
  }else{
    guitar.quantity = 1
    cart.value.push(guitar)
  }

  
};

const decrementQuantity = (id) => {
    const index = cart.value.findIndex(product => product.id === id)
    if(cart.value[index].quantity <= 1) return
    cart.value[index].quantity--

}

const incrementQuantity = (id) => {
  const index = cart.value.findIndex(product => product.id === id)
  if(cart.value[index].quantity >= 5) return
  cart.value[index].quantity++
  
}

const removeProduct = (id) => {
  cart.value = cart.value.filter( (product) => product.id !== id)
}

const deleteCart = () => {
  cart.value = []
}
</script>

<template>
  <Header 
    :cart="cart"
    :promGuitar="promGuitar"
    @decrement-quantity="decrementQuantity"
    @increment-quantity="incrementQuantity"
    @add-to-cart="addToCart"
    @remove-product="removeProduct"
    @delete-cart="deleteCart"
  />

  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>

    <div class="row mt-5">
      <Guitar
        v-for="guitar in guitars"
        :guitar="guitar"
        :key="guitar.id"
        @add-to-cart="addToCart"
              />
    </div>
  </main>

  <Footer />

</template>
