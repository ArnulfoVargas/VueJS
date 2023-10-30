<script setup>

</script>

<template>
    <form class="presupuesto">
        <div class="campo">
            <label for="nuevo-presupuesto">Definir Presupuesto</label>

            <input type="number"
            id="nuevo-presupuesto"
            class="nuevo-presupuesto"
            placeholder="Añade tu presupuesto"
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