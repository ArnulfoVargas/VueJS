<script setup>
    import {ref , onMounted, watch} from "vue"
    import {db} from "./data/data.js"
    import Guitarra from "./components/Guitarra.vue"
    import Header from "./components/header.vue"
    import Footer from "./components/Footer.vue"

    const guitarras = ref([])
    const carrito = ref([])
    const guitarraPrincipal = ref({})

    watch(carrito ,() => {
        guardar()
    }, {
        deep: true
    })

    onMounted(()=>{
        guitarras.value = db;
        guitarraPrincipal.value = db[3]

        const carritoStorage = JSON.parse(localStorage.getItem("carrito"))

        if(carritoStorage)
            carrito.value = carritoStorage
    })

    const onAddToCart = (guitarra) => {

        const existe = carrito.value.findIndex((producto) => producto.id === guitarra.id);

        if (existe < 0)
        {
            guitarra.cantidad = 1;
            carrito.value.push(guitarra)
        }
        else{
            if (guitarra.cantidad >= 5) return;

            guitarra.cantidad++;
        }
    }

    const decrementarCantidad = (item) => {
        item.cantidad--;

        if(item.cantidad <= 0)
            eliminarProduto(item);
    }

    const aumentarCantidad = (item) => {
        if(item.cantidad >= 10) return;

        item.cantidad++
    }

    const eliminarProduto = (item) => {
        carrito.value = carrito.value.filter((elemento) => elemento.id !== item.id)
    }

    const vaciarCarrito = () => {
        carrito.value = []
    }

    const guardar = () => {
        localStorage.setItem("carrito", JSON.stringify(carrito.value));
    }

</script>

<template>
  
    <Header
    v-bind:carrito="carrito"
    v-bind:guitarra-principal="guitarraPrincipal"

    v-on:on-add="aumentarCantidad"
    v-on:on-remove="decrementarCantidad"
    v-on:on-add-to-cart="onAddToCart"
    v-on:on-delete="eliminarProduto"
    v-on:on-empty-cart="vaciarCarrito"
    />

    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>

        <div class="row mt-5">

            <Guitarra 
            v-for="guitarra in guitarras" 
            v-bind:key="guitarra.id"
            v-bind:guitarra = "guitarra"
            
            v-on:on-add-to-cart = "onAddToCart"
            />

        </div>
    </main>

    <Footer/>

</template>

