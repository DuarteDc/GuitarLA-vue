<script setup>

import { ref, onMounted, watch } from 'vue';
import { db } from './data/guitars';

import Guitar from './components/Guitar.vue';
import Header from './components/Header.vue';
import Footer from './components/Footer.vue';


  const guitars = ref([]);
  const headerGuitar = ref({});
  const cart = ref([]);


  watch(cart, ()=> saveLocalStorage(), { deep: true });

  onMounted(() => {
    guitars.value = db;
    headerGuitar.value = db[1];

    const cartStorage = localStorage.getItem('__cart__');

    if(cartStorage) {
      cart.value = JSON.parse(cartStorage) || []
    }

  })

  const addCart = (guitar) => {

    const productInCart = cart.value.findIndex(product => product.id === guitar.id);

    if(productInCart >= 0) 
      return cart.value[productInCart].cantidad += 1;
      
    guitar.cantidad = 1;
    cart.value.push(guitar);

  }

  const saveLocalStorage = () => 
    localStorage.setItem('__cart__', JSON.stringify(cart.value));
  

  const decreaseQuiantity = (id) => {
    const index = cart.value.findIndex(product => product.id === id);
    if(cart.value[index].cantidad <=1 ) return;
    cart.value[index].cantidad -= 1;
  }

  const increaseQuantity = (id) => {
    const index = cart.value.findIndex(product => product.id === id);
    cart.value[index].cantidad += 1;
  }

  const deleteProduct = (id) => {
    cart.value = cart.value.filter(product => product.id !== id);
  }

  const cleanCart = () => {
    cart.value = [];
  }

</script>

<template>
  <Header
    :cart="cart"
    :headerGuitar="headerGuitar"
    @increase-quantity="increaseQuantity"
    @decrease-quantity="decreaseQuiantity"
    @add-cart="addCart"
    @delete-product="deleteProduct"
    @clean-cart="cleanCart"
  /> 
  <main class="container-xl mt-5">
    <h2 class="text-center my-10">Nuestra Colección</h2>
    <div class="row mt-5">

      <Guitar 
        v-for="guitar in guitars"
        :guitar="guitar"
        @add-cart="addCart"

      />

    </div>
  </main>


  <Footer />
</template>
