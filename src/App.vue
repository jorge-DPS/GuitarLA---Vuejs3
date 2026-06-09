<script setup>
import { ref, onMounted } from "vue";
import { db } from "./data/guitars";
import Guitar from "./components/Guitar.vue";
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";

const guitars = ref([]);
const cart = ref([]);

onMounted(() => {
  guitars.value = db;
});

const addToCart = (guitar) => {
  cart.value.push(guitar);
};
</script>

<template>
  <Header 
    :cart="cart"
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
