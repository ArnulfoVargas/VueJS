<script setup>
    import imagen from '../assets/img/grafico.jpg'
    import { formatearCantidad } from '../helpers'

    const props = defineProps({
        presupuesto: {
            type:Number,
            required: true
        },
        disponible: {
            type:Number,
            required: true
        }
    })

    const emits = defineEmits(["definir-presupuesto"])

    const definirPresupuesto = ()=>{
        emits('definir-presupuesto', 0)
    }
</script>

<template>
    <div class="dos-columnas">
        <div class="contenedor-grafico">
            <img 
                :src="imagen"
            />
        </div>

        <div class="contenedor-presupuesto">
            <button class="reset-app"
            @click="definirPresupuesto">
                Resetear App
            </button>
            <p>
                <span>Presupuesto: </span>
                {{ formatearCantidad(presupuesto) }}
            </p>

            <p>
                <span>Disponible: </span>
                {{ formatearCantidad(disponible) }}
            </p>
            <p>
                <span>Gastado: </span>
                $0
            </p>
        </div>
    </div>
</template>

<style scoped>
    .dos-columnas{
        display: flex;
        flex-direction: column;
    }
    .dos-columnas :first-child{
        margin-bottom: 2rem;
    }
    
    @media (min-width: 768px){
        .dos-columnas{
            flex-direction: row;
            gap: 4rem;
            align-items: center;
        }
        .dos-columnas :first-child{
        margin-bottom: 0;
        } 
    }

    .contenedor-presupuesto{
        width: 100%;
    }

    .reset-app{
        background-color: #db2748;
        border: none;
        padding: 1rem;
        width: 100%;
        border-radius: 1rem;
        color: var(--gris-claro);
        font-weight: 800;
        text-transform: uppercase;

        transition: background-color .25s linear;
    }

    .reset-app:hover{
        background-color: #c50024;
        cursor: pointer;
    }

    .contenedor-presupuesto p{
        font-size: 2.4rem;
        text-align: center;
        color: var(--gris-oscuro);
        font-weight: 600;
    }

    .contenedor-presupuesto span{
        font-weight: 900;
        color: var(--azul);
    }

    @media (min-width: 768px){
        .contenedor-presupuesto p{
        text-align: left;
    }
    }
</style>