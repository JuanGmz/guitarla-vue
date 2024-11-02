<script setup>
    import { ref } from 'vue'

    // Con define props se pueden pasar propiedades a los componentes de forma dinámica a través de un objeto.
    const props = defineProps({
        guitarra: {
            type: Object,
            required: true
        }
    })

    // Con defineEmits se puede emitir eventos a otros componentes de forma dinámica.
    const emit = defineEmits(['agregar-carrito'])
</script>

<template>
    <div class="col-md-6 col-lg-4 my-4 row align-items-center">
        <div class="col-4">
            <!-- Con v-bind se hace dinámico el atributo -->
            <img class="img-fluid" v-bind:src="'/img/' + guitarra.imagen + '.jpg'" v-bind:alt="'imagen ' + guitarra.nombre">
        </div>
        <div class="col-8">
            <h3 class="text-black fs-4 fw-bold text-uppercase">{{ guitarra.nombre }}</h3>
            <p>{{guitarra.descripcion}}</p>
            <p class="fw-black text-primary fs-3">${{ guitarra.precio }}</p>
            <!-- Con v-on se hace dinámico el evento y se emite el evento a otro componente -->
            <!-- Para pasar información al componente padre se envía en el emit -->
            <button 
                type="button"
                class="btn btn-dark w-100"
                v-on:click="$emit('agregar-carrito', guitarra)"
            >
            Agregar al Carrito</button>
        </div>
    </div>
</template>