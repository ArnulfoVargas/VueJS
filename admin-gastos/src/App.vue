<script setup>
import { ref, reactive, watch, computed, onMounted } from "vue"

import IconoNuevoGasto from "./assets/img/nuevo-gasto.svg"

import Presupuesto from './components/Presupuesto.vue';
import ControlPresupuesto from './components/ControlPresupuesto.vue';
import Gasto from './components/Gasto.vue'
import Modal from './components/Modal.vue'
import Filtro from './components/Filtro.vue'
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
const gastado = ref(0)
const gastos = ref([])
const filtro = ref("")

watch(gastos, () => {
  const totalGastado = gastos.value.reduce((total, gasto) => {
    return gasto.cantidad + total
  }, 0);

  gastado.value = totalGastado;
  disponible.value = presupuesto.value - totalGastado
  localStorage.setItem("gastos", JSON.stringify(gastos.value))
}, {
  deep: true
})

watch(presupuesto, () => {
  localStorage.setItem("presupuesto", presupuesto.value)
})

onMounted(() => {
  presupuesto.value = +localStorage.getItem("presupuesto") ?? 0
  gastos.value = JSON.parse(localStorage.getItem("gastos") ?? [])
})

const reiniciarGasto = () => {
  Object.assign(gasto, { 
    nombre: '',
    cantidad: '',
    categoria: '',
    id: null,
    fecha: Date.now(),
  });
}

const definirPresupuesto = (cantidad) => {
  presupuesto.value = cantidad;
  disponible.value = cantidad;
  gastos.value = []
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
    reiniciarGasto()
    modal.mostrar = false;
  }, 300)
}

const guardarGasto = () => {
  if (gasto.id != null) {
    const { id } = gasto
    const i = gastos.value.findIndex((e) => e.id === id)

    gastos.value[i] = {...gasto}
  }
  else {
    gastos.value.push({...gasto, id:generarID()})
  }
  cerrarModal()
}

const seleccionarGasto = (id) => {
  const gastoObtenido = gastos.value.filter(e => e.id === id)[0]
  Object.assign(gasto, gastoObtenido)

  mostrarModal()
}

const eliminarGasto = () => {
  gastos.value = gastos.value.filter(e => e.id != gasto.id)
  cerrarModal()
}

const gastosFiltrados = computed(() => {
  if (filtro.value) {
    return gastos.value.filter(gasto => gasto.categoria === filtro.value)
  }

  return gastos.value
})

const resetApp = () => {
  if (confirm('Deseas reiniciar el presupuesto y los gastos?')){
    gastos.value = []
    presupuesto.value = 0
  }
}
</script>

<template>
  <div
    :class="{fijar: modal.mostrar}"
  >
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
        :gastado="gastado"
        @reset-app="resetApp"
        />
      </div>
    </header>

    <main v-if="presupuesto > 0">
      <Filtro
        v-model:filtro="filtro"
      />

      <div class="listado-gastos contenedor">
        <h2>{{ gastos.length > 0 ? "Gastos" : "No hay gastos" }}</h2>
        <Gasto 
          v-for="gasto in gastosFiltrados" 
          :key="gasto.id" 
          :gasto="gasto"
          @seleccionar-gasto="seleccionarGasto"
          />
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
        :disponible="disponible"
        :id="gasto.id"
        @cerrar-modal="cerrarModal"
        @guardar-gasto="guardarGasto"
        @eliminar-gasto="eliminarGasto"
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

  .fijar {
    overflow: hidden;
    height: 100dvh;
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
