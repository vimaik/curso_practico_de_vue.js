<template>
    <div>
        <svg
            viewBox="0 0 300 200"
        >
            <line 
                stroke="#c4c4c4"
                stroke-width="2"
                x1="0"
                :y1="yZeroPoint"
                x2="300"
                :y2="yZeroPoint"
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
        <div>{{ yZeroPoint }}</div>
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

const amountToPixels = (amount) => {
    const minAmount = Math.min(...amounts.value);
    const maxAmount = Math.max(...amounts.value);

    const amountsRange = Math.abs(minAmount) + Math.abs(maxAmount);
    const amountAbs = amount + Math.abs(minAmount);

    // 200 es la altura fijada del viewBox del polyline, en el proyecto
    // Esto es un regla de 3 para calcular la equivalencia de valores
    // (importes) reales adaptados al viewBox:
    //
    //      amountRange <-> 200
    //        amountAbs <-> x
    const viewBoxAdaptedAmount = (amountAbs * 200) / amountsRange;

    // Como el viewBox va de 0 a -200, por su naturaleza, invertimos
    // el eje de los valores para que los importe postitivos crezcan y
    // los negativos decrezcan en el gráfico
    return 200 - viewBoxAdaptedAmount;
}

const yZeroPoint = computed(() => {
    return amountToPixels(0);
});

const points = computed(() => {
    const numPoints = amounts.value.length;
    // Array.prototype.reduce(funcion con los parámetros: variableAcumuladoraARetornar, valorActualdelArrayOriginal e indiceDelValorActualDelArrayOriginal' más ', valorInicialDeLaVariableAcumuladoraARetornar')
    return amounts.value.reduce((points, amount, i) => {
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