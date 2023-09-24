<script setup>
    import { reactive, computed } from 'vue';
    import Alerta from "./Alerta.vue"

    let timer;

    const emit = defineEmits([  'update:mascota', 'update:propietario', 'update:email', 
                                'update:alta', 'update:sintomas', 'guardar-paciente'])
    const props = defineProps({
        id      :   {
                        required: true,
                        type: [String, null]
                    },
        mascota :   {
                        required: true,
                        type: String
                    },
        propietario:{
                        required: true,
                        type: String
                    },
        email   :   {
                        required: true,
                        type: String
                    },
        alta    :   {
                        required: true,
                        type: String
                    },
        sintomas:   {
                        required: true,
                        type: String
                    }
    })

    const alerta = reactive(
        {
            tipo    : "",
            mensaje : ""
        }
    )

    const validar = _ => {

        if (timer)
            clearTimeout(timer)

        if (Object.values(props).includes("")){   

            alerta.tipo = "error"
            alerta.mensaje = "Todos los campos son obligatorios"

            timer = setTimeout(()=>{alerta.tipo = ''; alerta.mensaje = ''}, 2500) 

            return
        }

        alerta.tipo = "success"
        alerta.mensaje = "Guardado correctamente"

        timer = setTimeout(()=>{alerta.tipo = ''; alerta.mensaje = ''}, 2500) 

        emit('guardar-paciente')

        return
    }

    const buttonText = computed(() => { return props.id ? 'Aplicar Cambios' : 'Registrar Paciente'})
</script>

<template>
    <div class="md:w-1/2">

        <h2 class="font-black text-2xl text-center">Seguimiento Pacientes</h2>

        <p class="text-lg mt-5 text-center mb-10">
            Añade pacientes y 
            <span class="text-indigo-600 font-bold">Adminístralos</span>
        </p>

        <Alerta
            v-if="alerta.mensaje"
            :alerta = "alerta"
        />

        <form 
        class="shadow-md bg-white rounded-lg pt-10 pb-2 px-5 mb-10"
        v-on:submit.prevent="validar"
        >

            <div class="mb-5 "> 

                <label class="block text-gray-700 font-bold uppercase" for="mascota">
                    Nombre Mascota:
                </label>

                <input 
                    type="text" 
                    id="mascota" 
                    placeholder="Nombre de la mascota"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    autocomplete="none"
                    v-on:input="$emit('update:mascota', $event.target.value)"
                    :value="mascota"
                    >

            </div>

            <div class="mb-5 "> 

                <label class="block text-gray-700 font-bold uppercase" for="propietario">
                    Nombre Propietario:
                </label>

                <input 
                    type="text" 
                    id="propietario" 
                    placeholder="Nombre del propietario"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    v-on:input="$emit('update:propietario', $event.target.value)"
                    :value="propietario"
                    >
            </div>

            <div class="mb-5 "> 

                <label class="block text-gray-700 font-bold uppercase" for="email">
                    Email:
                </label>

                <input 
                    type="email" 
                    id="email" 
                    placeholder="Email del propietario"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    v-on:input="$emit('update:email', $event.target.value)"
                    :value="email"
                    >

            </div>

            <div class="mb-5 "> 

                <label class="block text-gray-700 font-bold uppercase" for="alta">
                    Alta:
                </label>
            
                <input 
                    type="date" 
                    id="alta" 
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    v-on:input="$emit('update:alta', $event.target.value)"
                    :value="alta"
                    >

            </div>

            <div class="mb-5 "> 

                <label class="block text-gray-700 font-bold uppercase" for="sintomas">
                    Sintomas:
                </label>

                <textarea
                    id="sintomas" 
                    placeholder="Sintomas"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md h-40"
                    :value="sintomas"
                    v-on:input="$emit('update:sintomas', $event.target.value)"
                    >
                </textarea>

                <input type="submit"
                    class="bg-indigo-600 hover:bg-indigo-700 transition-colors text-center
                     text-white w-full p-3 uppercase font-bold cursor-pointer mt-5 rounded-md"
                    :value="buttonText"
                >
            </div>

        </form>

    </div>

</template>