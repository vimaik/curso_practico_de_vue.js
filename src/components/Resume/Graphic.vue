<template>
    <div>
        <svg
            viewBox="0 0 300 200"
        >
            <line 
                stroke="#c4c4c4"
                stroke-width="2"
                x1="0"
                y1="100"
                x2="300"
                y2="100"
            />
            <polyline
                fill="none"
                stroke="#0689B0"
                stroke-width="2"
                :points="points"
            />
            <line 
                stroke="#04b500"
                stroke-width="2"
                x1="200"
                y1="0"
                x2="200"
                y2="200"
            />
        </svg>
        <p>Últimos 30 días</p>
        <p>{{ points }}</p>
    </div>

</template>

<script setup>
import { computed, defineProps, toRefs } from "vue";

const props = defineProps({
    amounts: {
        type: Array,
        default: () => []
    }
});

const { amounts } = toRefs(props);

const amountToPixels = () => {
    const minAmount = Math.min(...amounts.value);
    const maxAmount = Math.max(...amounts.value);

    return `${minAmount},${maxAmount}`;
}

const points = computed(() => {
    const numPoints = amounts.value.length;
    // Array.prototype.reduce(funcion con los parámetros: variableAcumuladoraARetornar, valorActualdelArrayOriginal e indiceDelValorActualDelArrayOriginal' más ', valorInicialDeLaVariableAcumuladoraARetornar')
    return Array(numPoints).fill(100).reduce((points, amount, i) => {
        const x = (300/ numPoints) * (i + 1);
        const y = amountToPixels(amount);
        console.log(y);
        return  `${points} ${x},${y}`
    }, "0,100");
});
</script>

<style scoped>
svg {
    width: 100%;
}

p {
    text-align: center;
}
</style>