<script setup>
  import { ref, reactive, onMounted } from 'vue'
  import Header from './components/Header.vue'
  import Footer from './components/Footer.vue'
  import Guitarra from './components/Guitarra.vue'
  import { db } from './data/guitarras.js'

  // reactive Siempre es un objeto, para acceder a las propiedades es con un “.”, para modificarlo sería con “.” también.
  // const state = reactive({
  //     guitarras : db
  // })

  // ref Funciona para Arrays, Booleans y Strings, cuándo se declara un ref se tiene que colocar un valor inicial, para acceder a una propiedad es con “.value” para reescribir es igual.
  const guitarras = ref([])

  // Con onMounted una vez que el componente este listo se va a cargar y a ejecutar.
  onMounted(() => {
    // reactive
    // state.guitarras = db

    // ref
    guitarras.value = db
  })

  const agregarCarrito = (guitarra) => {
    console.log('Se agrego la guitarra ', guitarra)
  }
</script>

<template>
  <!-- Componente de header -->
  <Header />
  <!-- Fin Componente -->

  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>

    <div class="row mt-5">
      <!-- Componente de guitarra, v-bind para pasar las propiedades a otros componentes, se puede escribir v-bind o ":" -->
      <!-- v-on para escuchar eventos, en este caso se escucha el evento “incrementar” y se ejecuta la función “incrementar” -->
      <Guitarra 
        v-for="guitarra in guitarras"
        v-bind:guitarra="guitarra"
        v-on:agregar-carrito="agregarCarrito"
      />
      <!-- Fin Componente -->
    </div>
  </main>

  <!-- Componente de footer -->
  <Footer />
  <!-- Fin Componente -->
</template>