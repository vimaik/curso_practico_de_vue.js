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
import {
    computed,
    defineEmits,
    defineProps,
    ref,
    toRefs,
    watch
} from "vue";

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

    // 200 es la altura que hemos fijado enn el elemento viewBox que contiene
    // la polilinea (polyline) que represetna el importe de los movimientos.
    // Para adaptar los importe reales al tamaño de la caja que contiene la gráfica
    // (tamaño del viewBox aplicamos una regla de 3, que nos dará la equivalencia
    // de los direrentes valores (importes) reales adaptados al viewBox:
    //
    //      amountRange <-> 200
    //        amountAbs <-> x
    const viewBoxAdaptedAmount = ((amountAbs * 100) / amountsRange) * 2;

    // Hemos establecido que el alto del elemento que contiene la gráfica (viewBox)
    // sea de 200. Por la naturaleza (como está programado) 'del elemento viewBox',
    // debemois tener en cuenta de que en realidad su eje 'Y' varia de 0 a -200,
    // y no de 0 a 200, así que, para que los importes postitivos crezcan y los negativos
    // decrezcan en el gráfico, invertimos el eje Y mediante la siguiente operación.
    return 200 - viewBoxAdaptedAmount;
}

// Como el viewBox que contien la gráfica de importes va desde0 a -200,
// El punto 0 de l eje 'Y' de la gráfica está en -100
const yZeroPoint = computed(() => {
    return amountToPixels(-100);
});

const points = computed(() => {
    const numPoints = amounts.value.length;
    // Parámetros de Array.prototype.reduce((a, b, c) => {}, d);
    //     a: variableAcumuladoraADevolver
    //     b: valorActualdelArrayOriginal
    //     c: indiceDelValorActualDelArrayOriginal
    //     d: valorInicialDeLaVariableAcumuladoraADevolver
    return amounts.value.reduce((points, amount, i) => {
        const x = (300/ numPoints) * (i + 1);
        const y = amountToPixels(amount);
        return  `${points} ${x},${y}`;
    }, `0,${amountToPixels(amounts.value.length ? amounts.value[0] : 0)}`);
});

const showPointer = ref(false);
const pointer = ref(0);

const emit = defineEmits(["select"]);

watch(pointer, (value) => {
    // Calculamos el ancho (trozo del eje 'X'), en píxeles, de nuestra gráfica
    // correspondiente a lo que realmente ocupa CADA MOVIMIENTO dentro de la
    // la caja que contiene nuestra gráfica (el elemento 'viewBox', cuya anchura
    // total hemos fijado previamente a 300 pixeles), y lo redondeamos a píxeles
    // enteros mediante la función Math.ceil(). Con esto obtenemos el índice del
    // elemento del array 'amounts' que hemos seleccionado.
    const index = Math.ceil((value / (300 / amounts.value.length)));

    // Antes, sin embargo, tenemos que excluir (no hacer nada) si se selecciona
    // cualquier punto de la pantalla del dispositivo que no pertenezcan al
    // viewBox que contiene la gráfica de importes.
    if (index < 0 || index > amounts.value.length ) return;

    // Si se ha pulsado una coordenada correcta (correspondiente a un movimiento
    // de la gráfica), pasamos al elemento padre (Home.vue) el importe del
    // movimiento (indice) seleccionado del array de importes ('amounts').
    emit("select", amounts.value[index]);
});

const tap = ({ target, touches }) => {
    showPointer.value = true;
    
    // 'screenWidth' es el ancho de la pantalla del dispositivo que está utilizando
    // la aplicación (SmartPhone, tablet, ...)
    const screenWidth = target.getBoundingClientRect().width;

    // Coordenada X dónde comienza el elemento SVG (viewBox, o lo que ocupa la caja
    // que contiene el gráfico de importes) dentro de la pantalla del dispositivo
    // (SmartPhone, tablet, ...).
    const svgInitialXPoint = target.getBoundingClientRect().x;

    // Coordenada X dónde se ha hecho el touch
    const touchXPoint = touches[0].clientX;

    // En este proyecto, 300 es el ancho fijado del viewBox del polyline
    // Esto es un regla de 3 para calcular la equivalencia de valores
    // (importes) reales adaptados al viewBox:
    //
    //                       screenWidth <-> 300
    //    touchXPoint - svgInitialXPoint <-> x
    pointer.value = ((touchXPoint - svgInitialXPoint) * 300) / screenWidth;
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