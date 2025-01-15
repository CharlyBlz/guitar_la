<script setup>
  import {ref, reactive, onMounted, watch} from 'vue'
  import {db} from './data/guitarras'
  import Guitarra from './components/Guitarra.vue'
  import Header from './components/Header.vue'
  import Footer from './components/Footer.vue'
  
  // STATES
  const guitarras = ref([])
  const carrito = ref([])
  // Guitarra es un objeto
  const guitarra = ref({})
  
  // WATCH, donde carrito es la dependencia a observar, luego tiene un callback, "deep" entra a
  watch(carrito, () => {
    guardarLocalStorage()
  },{
    deep: true
  })


  // MÉTODO DE CICLO DE VIDA
  onMounted(() => {
    guitarras.value = db
    guitarra.value = db[3]
    const carritoStorage = localStorage.getItem('carrito')
      if(carritoStorage){
        carrito.value = JSON.parse(carritoStorage)
      }
  })

  // FUNCIÓN PARA GUARDAR EN ALMACENAMIENTO LOCAL
  const guardarLocalStorage = () => {
    localStorage.setItem('carrito', JSON.stringify(carrito.value))
  } 

  const agregarCarrito = (guitarra) => {
    const exiteCarrito = carrito.value.findIndex(producto => producto.id === guitarra.id)
    if(exiteCarrito >= 0){
      carrito.value [exiteCarrito].cantidad++
    } else {
      guitarra.cantidad = 1
      carrito.value.push(guitarra)
    }
    
  }
  // Se identifica la posición en el arreglo de ese elemento y únicamente reescribe ese elemento.
  const decrementarCantidad = (id) => {
    const index = carrito.value.findIndex ( producto => producto.id === id )
    if(carrito.value[index].cantidad <= 1) return
    carrito.value[index].cantidad--

    }
    
  
  
  const incrementarCantidad = (id) => {
    const index = carrito.value.findIndex ( producto => producto.id === id )
    if(carrito.value[index].cantidad >= 5) return
    carrito.value[index].cantidad++

  }

  const eliminarProducto = (id) => {
    carrito.value = carrito.value.filter(producto => producto.id !== id)

  }

  const vaciarCarrito = () => {
    carrito.value = []

  }
</script>


<template>
  <!--:carrito (es el prop) ="carrito" (valor que se está pasando)-->
  <!-- Se registra un CUSTOM EVENT en Header -->
  <!-- Dos puntos (:) para un PROP y @ para un EVENTO-->
  <!-- PROPS arriba y EVENTOS abajo -->
  <Header
    :carrito="carrito"
    :guitarra="guitarra"
    @incrementar-cantidad="incrementarCantidad"
    @decrementar-cantidad="decrementarCantidad"
    @agregar-carrito="agregarCarrito"
    @eliminar-producto="eliminarProducto"
    @vaciar-carrito="vaciarCarrito"
  /> 

  <main class="container-xl mt-5">
      <h2 class="text-center">Nuestra Colección - Actualizada</h2>

      <div class="row mt-5"> 
          <!-- SE RENDERIZA COMPONENTE GUITARRA-->
          <!-- v-bind:guitarra = "guitarra" ES LO MISMO QUE :guitarra = "guitarra"-->
          <!-- v-for = "guitarra in guitarras" SE ITERA SOBRE EL STATE DE guitarras-->
          <!-- :guitarra = "guitarra" SE ESTÁ PASANDO VÍA PROPS EL OBJETO DE guitarra hacia el componente Guitarra.vue-->
            <!-- SE ENVÍAN DATOS DE COMPONENTE PADRE App.vue A COMPONENTE HIJO Guitarra.vue MEDIANTE PROPS-->
          <Guitarra 
              v-for = "guitarra in guitarras"
              :guitarra = "guitarra"
              @agregar-carrito = "agregarCarrito"
          />
      </div>
  </main>

  <Footer/>
    
</template>

