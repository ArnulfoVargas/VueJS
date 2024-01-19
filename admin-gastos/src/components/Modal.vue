<script setup>
    import { ref } from "vue";
    import Alerta from "./Alerta.vue"
    import cerrarModalIcono from "../assets/img/cerrar.svg"

    const emits = defineEmits(["cerrar-modal", "update:nombre", "update:categoria", "update:cantidad", "guardar-gasto", "eliminar-gasto"])
    const props = defineProps({
        modal:{
            type:Object,
            required: true,
        },
        nombre:{
            type:String,
            required:true
        },
        cantidad:{
            type:[Number, String],
            required: true
        },
        categoria:{
            type:String,
            required: true
        },
        disponible:{
            type: Number,
            required: true
        },
        id:{
            type: [String, null],
            required: true
        },
    })

    const error = ref("")
    let timer;
    const cantidadPrevia = props.cantidad;

    const agregarGasto = () => {
        const { nombre, cantidad, categoria, disponible, id } = props

        if ([nombre.trim(), cantidad, categoria].includes('')){
            error.value = "Todos los campos son obligatorios"

            if (timer)
                clearTimeout(timer)

            timer = setTimeout(() => {error.value = ""}, 3000)

            return;
        }
        if (cantidad <= 0) {
            error.value = "La cantidad debe de ser mayor a 0"

            if (timer)
                clearTimeout(timer)

            timer = setTimeout(() => {error.value = ""}, 3000)

            return;
        }

        if (id) {
            if (cantidad > cantidadPrevia + disponible) {
                error.value = "Has excedido el Presupuesto"

                if (timer)
                    clearTimeout(timer)

                timer = setTimeout(() => {error.value = ""}, 3000)
            }
        } else {
            if (cantidad > disponible) {
                error.value = "Has excedido el Presupuesto"

                if (timer)
                    clearTimeout(timer)

                timer = setTimeout(() => {error.value = ""}, 3000)

                return;
            }
        }

        emits("guardar-gasto")
    }
</script>

<template>
    <div class="modal">
        <div class="cerrar-modal">
            <img 
                :src="cerrarModalIcono" 
                alt="Cerrar modal"
                @click="$emit('cerrar-modal')"
                >
        </div>

        <div 
            class="contenedor contenedor-formulario"
            :class="modal.animar ? 'animar' : 'cerrar'"
            >
            <form class="nuevo-gasto"
                @submit.prevent="agregarGasto">

                <legend>{{ id ? "Editar Gasto" : "Agregar Gasto"}}</legend>

                <Alerta v-if="error">{{ error }}</Alerta>

                <div class="campo">
                    <label for="nombre">Nombre del Gasto: </label>
                    <input 
                        type="text" 
                        id="nombre" 
                        placeholder="Agrega el nombre del gasto"
                        :value="nombre"
                        @input="$emit('update:nombre', $event.target.value)"
                        autocomplete="off"
                        >
                </div>
                <div class="campo">
                    <label for="cantidad">Cantidad: </label>
                    <input 
                        type="number" 
                        id="cantidad" 
                        placeholder="Agrega la cantidad del gasto"
                        :value="cantidad"
                        @input="$emit('update:cantidad', +$event.target.value)"
                        autocomplete="off"
                        >
                </div>
                <div class="campo">
                    <label for="categoria">Categoria: </label>
                    <select id="categoria" :value="categoria" @input="$emit('update:categoria', $event.target.value)">
                        <option value="">-- Seleccione --</option>
                        <option value="ahorro">Ahorro</option>
                        <option value="comida">Comida</option>
                        <option value="casa">Casa</option>
                        <option value="gastos">Gastos</option>
                        <option value="ocio">Ocio</option>
                        <option value="salud">Salud</option>
                        <option value="subscripciones">Subscripciones</option>
                    </select>
                </div>

                <input type="submit" :value="[id ? 'Guardar Cambios' : 'Agregar Gasto']">
            </form>

            <button
                type="button"
                class="btn-eliminar"
                @click="$emit('eliminar-gasto')"
                v-if="id"
                >
                Eliminar Gasto
            </button>
        </div>
    </div>
</template>

<style scoped>
    .modal{
        position: absolute;
        background-color: rgb(0 0 0 /0.9);
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
    }

    .cerrar-modal{
        position: absolute;
        right: 3rem;
        top: 3rem;
        width: 3rem;
        height: 3rem;
    }

    .cerrar-modal img{
        width: 3rem;
        cursor: pointer;
    }

    .contenedor-formulario{
        transition: all 300ms ease-in;
        opacity: 0;
    }

    .contenedor-formulario.animar{
        transition: all 300ms ease-in;
        opacity: 1;
    }

    .contenedor-formulario.cerrar{
        transition: all 300ms ease-in;
        opacity: 0;
    }

    .nuevo-gasto{
        margin: 10rem auto 0 auto;
        display: grid;
        gap: 2rem;
    }

    .nuevo-gasto legend{
        text-align: center;
        color: var(--blanco);
        font-weight: 700;
        font-size: 3rem;
    }
    .campo{
        display: grid;
        gap: 2rem;
    }

    .nuevo-gasto input, .nuevo-gasto select{
        background-color: var(--gris-claro);
        border-radius: 1rem;
        padding: 1rem;
        border: none;
        font-size: 2.2rem;
    }

    .nuevo-gasto label{
        color: var(--blanco);
        font-size: 3rem;
    }

    .nuevo-gasto input[type="submit"]{
        background-color: var(--azul);
        color: var(--blanco);
        font-weight: 700;
        cursor: pointer;

        transition: background-color 150ms linear;
    }
    .nuevo-gasto input[type="submit"]:hover{
        background-color: rgb(31, 89, 182);
    }

    .btn-eliminar{
        padding: 1rem;
        width: 100%;
        background-color: #ef4444;
        font-weight: 700;
        color: var(--blanco);
        font-size: 1.2rem;
        margin-top: 10rem;
        cursor: pointer;
        border: none;
    }
</style>