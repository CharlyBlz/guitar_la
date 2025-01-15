<script setup>
    // import {ref} from 'vue' // SE IMPORTA REF PARA TENER UN STATE

    // const numero = ref(0)   // SE INICIA EL STATE EN 0

    const props = defineProps({
        guitarra : {
            type: Object,
            required: true
        }
    })

    defineEmits(['agregar-carrito'])

</script>

<template>
    <div class="col-md-6 col-lg-4 my-4 row align-items-center">
        <div class="col-4">   <!--v-bind:src="guitarra.imagen"-->
            <img 
            class="img-fluid" 
            :src="'/img/' + guitarra.imagen + '.jpg'" 
            :alt=" 'imagen guitarra' + guitarra.nombre">
        </div>
        <div class="col-8">
            <h3 class="text-black fs-4 fw-bold text-uppercase">{{ guitarra.nombre}}</h3>
            <!--<p>{{ numero }}</p> -->
            <!-- SE IMPRIME EL STATE EN UN PÁRRAFO EN EL TEMPLATE, ES DECIR, LA PRESENTACIÓN -->
            <p>{{guitarra.descripcion}}</p>
            <p class="fw-black text-primary fs-3">${{ guitarra.precio }}</p>
            <button 
                type="button"
                class="btn btn-dark w-100 "
                @:click = "$emit('agregar-carrito', guitarra)"
            >Agregar al Carrito</button> <!-- @ PARA EVENTOS EN COMPONENTES -->
        </div>
    </div><!-- FIN GUITARRA -->
</template>


<style lang="scss" scoped>

</style>