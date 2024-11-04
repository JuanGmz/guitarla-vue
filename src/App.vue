<script setup>
  import { ref, reactive, onMounted, watch } from 'vue'
  import Header from './components/Header.vue'
  import Footer from './components/Footer.vue'
  import Guitarra from './components/Guitarra.vue'
  import { db } from './data/guitarras.js'

  // reactive Siempre es un objeto, para acceder a las propiedades es con un “.”, para modificarlo sería con “.” también.
  // const state = reactive({
  //     guitarras : db
  // })

  // states
  // ref Funciona para Arrays, Booleans y Strings, cuándo se declara un ref se tiene que colocar un valor inicial, para acceder a una propiedad es con “.value” para reescribir es igual.
  const guitarras = ref([])
  const carrito = ref([])
  const guitarra = ref({})

  // El primer paramétro que se le pasa es el state el cuál va a estar observando para cambios
  watch(carrito, () => {
    // Cuando se agregue un elemento se guardará en localstorage
    localStorageCarrito()
  }, {
    // revisa contenido por contenido del arreglo u objeto y observa si cambia
    deep: true
  })

  // Con onMounted una vez que el componente este listo se va a cargar y a ejecutar.
  onMounted(() => {
    // reactive
    // state.guitarras = db

    // ref
    guitarras.value = db
    guitarra.value = db[3]
    // traer el valor del carrito de la funcion localstorage para poder recuperar los items guardados
    const carritoStorage = localStorage.getItem('carrito')
    
    // Validar que haya items en el carrito y convertir lo que hay a un arreglo
    if (carritoStorage) {
      carrito.value = JSON.parse(carritoStorage)
    }
  })

  const localStorageCarrito = () => {
    // Almacenar el valor del carrito en un JSON en localstorage, recibe el nombre con el que se recupera y el valor a guardar 
    localStorage.setItem('carrito', JSON.stringify(carrito.value))
  }

  const agregarCarrito = (guitarra) => {
    // Buscar si la guitarra ya existe en el carrito
    const existeCarrito = carrito.value.findIndex(producto => producto.id === guitarra.id)

    if (existeCarrito >= 0) {
      carrito.value[existeCarrito].cantidad++
    } else {
      // Cantidad de la guitarra al agregar al carrito
      guitarra.cantidad = 1
      // Agregar la guitarra al carrito
      carrito.value.push(guitarra)
    }
  }

  const decrementarCantidad = (id) => {
    // Buscar la guitarra en el carrito
    const index = carrito.value.findIndex(producto => producto.id === id)
    // Validar que la cantidad no sea menor a 1
    if (carrito.value[index].cantidad <= 1) return
    // Decrementar la cantidad
    carrito.value[index].cantidad--
  }

  const incrementarCantidad = (id) => {
    // Buscar la guitarra en el carrito
    const index = carrito.value.findIndex(producto => producto.id === id)
    // Validar que la cantidad no sea mayor a 5
    if (carrito.value[index].cantidad >= 5) return
    // Incrementar la cantidad
    carrito.value[index].cantidad++
  }

  const eliminarProducto = (id) => {
    // Eliminar el carrito con filter que recorre el array y retorna un nuevo array eliminando el que coincida con el id
    carrito.value = carrito.value.filter(producto => producto.id !== id)
  }

  const vaciarCarrito = () => {
    carrito.value = []
  }
</script>

<template>
  <!-- Componente de header -->
  <Header 
    v-bind:carrito="carrito"
    v-bind:guitarra="guitarra"
    v-on:decrementar-cantidad="decrementarCantidad"
    v-on:incrementar-cantidad="incrementarCantidad"
    v-on:agregar-carrito="agregarCarrito"
    v-on:eliminar-producto="eliminarProducto"
    v-on:vaciar-carrito="vaciarCarrito"
  />
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