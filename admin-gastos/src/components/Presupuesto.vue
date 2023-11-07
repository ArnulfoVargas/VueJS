<script setup>
    import {ref} from 'vue'
    import Alerta from './Alerta.vue';

    const emits = defineEmits(["definir-presupuesto"])

    const presupuesto = ref(0)
    const error = ref('')
    let timeOut;

    const definirPresupuesto = () => {
        if(presupuesto.value <= 0 || presupuesto.value == ''){
            error.value = "Ingresa un numero mayor a 0"

            if(timeOut)
                clearTimeout(timeOut)

            timeOut = setTimeout(()=> {error.value = ""}, 3000)

            return
        }
        emits('definir-presupuesto', presupuesto.value)
    }
</script>

<template>
    <form 
    class="presupuesto"
    @submit.prevent="definirPresupuesto"
    >
        <Alerta v-if="error">
            <p>{{error}}</p>
        </Alerta>

        <div class="campo">
            <label for="nuevo-presupuesto">Definir Presupuesto</label>

            <input type="number"
            id="nuevo-presupuesto"
            class="nuevo-presupuesto"
            placeholder="Añade tu presupuesto"
            min="0"
            v-model.number="presupuesto"
            />
        </div>  
        
        <input type="submit" value="Definir Presupuesto" />
    </form>
</template>

<style scoped>
    .presupuesto{
        width: 100%;
    }
    .campo{
        display: grid;
        gap: 2rem;
    }

    .presupuesto input[type="number"]{ 
        background-color: var(--gris-claro);
        border-radius: 1rem;
        padding: 1rem;
        border: none;
        font-size: 2.2rem;
        text-align: center;
    }

    .presupuesto input[type="submit"]{ 
        background-color: var(--azul);
        border: none;
        padding: 1rem;
        text-align: center;
        font-size: 2rem;
        margin-top: 2rem;
        border-radius: 1rem;
        color: var(--blanco);
        font-weight: 700;
        width: 100%;
        cursor: pointer;

        transition: background-color 300ms ease;
    }
    .presupuesto input[type="submit"]:hover{ 
        background-color: #1048a4;
    }

    .presupuesto label{
        font-size: 2.2rem;
        text-align: center;
        color: var(--azul);
    }
</style>