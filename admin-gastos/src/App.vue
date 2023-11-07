<script setup>
import { ref, reactive } from "vue"

import IconoNuevoGasto from "./assets/img/nuevo-gasto.svg"

import Presupuesto from './components/Presupuesto.vue';
import ControlPresupuesto from './components/ControlPresupuesto.vue';
import Modal from './components/Modal.vue'
import Gasto from './components/Gasto.vue'
import { generarID } from './helpers/index'

const modal = reactive({
  mostrar: false,
  animar : false,
})

const gasto = reactive({
  nombre: '',
  cantidad: '',
  categoria: '',
  id: null,
  fecha: Date.now(),
})
const presupuesto = ref(0)
const disponible = ref(0)
const gastos = ref([])

const definirPresupuesto = (cantidad) => {
  presupuesto.value = cantidad;
  disponible.value = cantidad;
}

const mostrarModal = () => {
  modal.mostrar = true;

  setTimeout(() => {
    modal.animar = true;
  }, 300)
}

const cerrarModal = () => {
  modal.animar = false;

  setTimeout(() => {
    modal.mostrar = false;
  }, 300)
}

const guardarGasto = () => {
  gastos.value.push({...gasto, id:generarID()})
  Object.assign(gasto, { 
    nombre: '',
    cantidad: '',
    categoria: '',
    id: null,
    fecha: Date.now(),
  });
  cerrarModal()
}

</script>

<template>
  <div>
    <header>
      <h1>Planificador de Gastos</h1>


      <div class="contenedor-header contenedor sombra">
        <Presupuesto
        @definir-presupuesto="definirPresupuesto"
        v-if="presupuesto === 0"
        />

        <ControlPresupuesto
        v-else
        :presupuesto="presupuesto"
        :disponible="disponible"
        @definir-presupuesto="definirPresupuesto"
        />
      </div>
    </header>

    <main v-if="presupuesto > 0">

      <div class="listado-gastos contenedor">
        <h2>{{ gastos.length > 0 ? "Gastos" : "No hay gastos" }}</h2>
        <Gasto 
          v-for="gasto in gastos" 
          :key="gasto.id" 
          :gasto="gasto"/>
      </div>

      <div class="crear-gasto">
        <img :src="IconoNuevoGasto"
              alt="icono nuevo gasto"
              @click="mostrarModal"
              />
      </div>

      <Modal
        v-if="modal.mostrar"
        :modal="modal"
        @cerrar-modal="cerrarModal"
        @guardar-gasto="guardarGasto"
        v-model:nombre="gasto.nombre"
        v-model:cantidad="gasto.cantidad"
        v-model:categoria="gasto.categoria"
      />
    </main>
  </div>
</template>

<style>
  :root{
    --azul: #3b82f6;
    --blanco: #FFF;
    --gris-claro: #f5f5f5;
    --gris: #94a3b8;
    --gris-oscuro:#64748b;
    --negro : #000;
  }

  html{
    font-size: 62.5%;
    box-sizing: border-box;
  }

  *, *::after, *::before{
    box-sizing: inherit;
  }

  body{
    font-size: 1.6rem;
    font-family: 'Lato', sans-serif;
    background-color: var(--gris-claro);
  }

  h1{
    font-size: 4rem;
  }
  h2{
    font-size: 3rem;
  }
  header{
    background-color: var(--azul);
  }

  header h1{
    padding: 3rem 0;
    margin: 0;
    color: var(--blanco);
    text-align: center;
  }

  .contenedor{
    width: 90%;
    max-width: 80rem;
    margin: 0 auto;
  }

  .contenedor-header{
    margin-top: -5rem;
    transform: translateY(5rem);
    padding: 5rem;
  }

  .sombra{
    box-shadow: 0px 10px 15px -3px rgba(0,0,0,0.1);
    background-color: var(--blanco);
    border-radius: 1.2rem;
    padding: 5rem;
  }

  .crear-gasto{
    position: fixed;
    bottom: 5rem;
    right: 5rem;
  }

  .crear-gasto img{
    width: 5rem;
    cursor: pointer;
    filter: saturate(1);
    transition: filter 150ms linear;
  }

  .crear-gasto img:hover{
    filter: saturate(.70);
  }

  .listado-gastos{
    margin-top: 10rem;
  }

  .listado-gastos h2{
    font-weight: 900;
    color: var(--gris-oscuro);
  }
</style>
