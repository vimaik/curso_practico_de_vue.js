<template>
    <div>
        <svg
            @touchstart="tap"
            @touchmove="tap"
            @touchend="untap"
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
                v-show="showPointer"
                stroke="#04b500"
                stroke-width="2"
                :x1="pointer"
                y1="0"
                :x2="pointer"
                y2="200"
            />
        </svg>
        <p>Últimos 30 días</p>
    </div>

</template>

<script setup>
import { computed, defineProps, ref, toRefs } from "vue";

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

    const amountAbs = amount + Math.abs(minAmount);
    const amountsRange = Math.abs(maxAmount) + Math.abs(minAmount);

    // 200 es la altura fijada del viewBox del polyline, en el proyecto
    // Esto es un regla de 3 para calcular la equivalencia de valores
    // (importes) reales adaptados al viewBox:
    //
    //      amountRange <-> 200
    //        amountAbs <-> x
    const viewBoxAdaptedAmount = ((amountAbs * 100) / amountsRange) * 2;

    // Como el elemento viewBox va de 0 a 200, por su naturaleza, invertimos
    // el eje de los valores para que los importe postitivos crezcan y
    // los negativos decrezcan en el gráfico
    return 200 - viewBoxAdaptedAmount;
}

const yZeroPoint = computed(() => {
    return amountToPixels(0);
});

const points = computed(() => {
    const numPoints = amounts.value.length;
    // Parámetros de Array.prototype.reduce((a, b, c) => {}, d);
    //     a: variableAcumuladoraARetornar
    //     b: valorActualdelArrayOriginal
    //     c: indiceDelValorActualDelArrayOriginal
    //     d: valorInicialDeLaVariableAcumuladoraARetornar
    return amounts.value.reduce((points, amount, i) => {
        const x = (300/ numPoints) * (i + 1);
        const y = amountToPixels(amount);
        return  `${points} ${x},${y}`;
    }, `0,${amountToPixels(amounts.value.length ? amounts.value[0] : 0)}`);
});

const showPointer = ref(false);
const pointer = ref(0);

const tap = ({ target, touches }) => {
    showPointer.value = true;
    // Ancho de la pantalla del dispositivo que utiliza la aplicación
    // (SmartPhone, tablet, ...)
    const screenWidth = target.getBoundingClientRect().width;

    // Coordenada X dónde comienza el elemento SVG (el gráfico de importes)
    // dentro de la pantalla del dispositivo (SmartPhone, tablet, ...)
    const svgInitialXPoint = target.getBoundingClientRect().x;

    // Coordenada X dónde se ha hecho el touch
    const touchXPoint = touches[0].clientX;

    // En este proyecto, 300 es el ancho fijado del viewBox del polyline
    // Esto es un regla de 3 para calcular la equivalencia de valores
    // (importes) reales adaptados al viewBox:
    //
    //                       screenWidth <-> 300
    //    touchXPoint - svgInitialXPoint <-> x
    pointer.value = ((touchXPoint - svgInitialXPoint) * 300) / screenWidth ;
}

const untap = () => {
    showPointer.value = false;
}
</script>

<style scoped>
svg {
    width: 100%;
}

p {
    text-align: center;
}
</style>