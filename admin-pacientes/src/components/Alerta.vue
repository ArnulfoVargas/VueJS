<script setup>
    import { computed } from 'vue';

    const props = defineProps(
        {alerta: {
            type: Object,
            required: true
        }}
    )

    const isError = computed( () => { return props.alerta.tipo === "error" })
</script>


<template>
    <div 
    :class="[isError ? 'bg-red-500' : 'bg-green-400']"
    class="text-center text-white p-3 uppercase font-bold mb-3 rounded-md"
    >
        <p>{{ alerta.mensaje }}</p>
    </div>
</template>
