<script setup>
  import { ref, reactive, watch, onMounted} from 'vue';

  import {uid} from 'uid'
  
  import Header from './components/Header.vue';
  import Formulario from './components/Formulario.vue';
  import Paciente from './components/Paciente.vue'

  
  const pacientes = ref([])
  
  const datos = reactive(
        {
            id                : null,
            nombreMascota     : "",
            nombrePropietario : "",
            email             : "",
            alta              : "",
            sintomas          : "",
        }
    )

  watch(pacientes, () => {
    localStorage.setItem('pacientes', JSON.stringify(pacientes.value))
    }, {deep: true})
  
  onMounted(() => {
    const data = JSON.parse(localStorage.getItem('pacientes'))
    if(data)
      pacientes.value = data;
  })
  
  const guardarPaciente = () => {
    if(datos.id){
      const i = pacientes.value.findIndex(e => e.id === datos.id)
      pacientes.value[i] = {...datos}
    }
    else{
      pacientes.value.push({...datos, id: uid()})
    }
    Object.assign(datos,  {
                            id                : null,
                            nombreMascota     : "",
                            nombrePropietario : "",
                            email             : "",
                            alta              : "",
                            sintomas          : "",
                          });
  }

  const actualizarPaciente = (paciente) => {
    Object.assign(datos, paciente)
  }

  const eliminarPaciente = (id) => {
    pacientes.value = pacientes.value.filter(e => e.id !== id)
  }

</script>

<template>
  
  <div class="container mx-auto mt-20">
    <Header/>

    <div class="mt-12 md:flex ">

      <Formulario 
        v-model:mascota="datos.nombreMascota"
        v-model:propietario="datos.nombrePropietario"
        v-model:email="datos.email"
        v-model:alta="datos.alta"
        v-model:sintomas="datos.sintomas"
        v-bind:id="datos.id"
        v-on:guardar-paciente="guardarPaciente"
      />

      <div class="md:w-1/2 md:h-screen overflow-y-scroll">
        <h2 class="font-black text-2xl text-center">Administración Pacientes</h2>

        <p class="text-lg mt-5 text-center mb-10">
            Información de
            <span class="text-indigo-600 font-bold">Pacientes</span>
        </p>

        <div v-if="pacientes.length > 0">
            <Paciente
              v-for="paciente in pacientes"
              v-bind:paciente = 'paciente'
              v-on:eliminar-paciente="eliminarPaciente"
              v-on:actualizar-paciente = 'actualizarPaciente'
            />
        </div>

        <p v-else class="mt-10 text-2xl text-center">No hay pacientes</p>
      </div>
      
      
    </div>


  </div>


</template>

