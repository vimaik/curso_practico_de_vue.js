<template>
   <main>
    <p class="label">{{ showedLabel }}</p>
    <h1>{{ showedAmountWithCurrency }}</h1>
    <div class="graphic">
        <slot name="graphic"></slot>
    </div>
    <div class="action">
        <slot name="action"></slot>
    </div>
   </main> 
</template>

<script>
const currencyFormatter = new Intl.NumberFormat(
    "es-ES",
    {
        style: "currency",
        currency: "EUR",
    }
);

export default {
    props: {
        totalLabel: {
            type: String,
            default: "Ahorro total",
        },
        label: {
            type: String,
            required: null,
        },
        totalAmount: {
            type: Number,
            default: 0,
        },
        amount: {
            type: Number,
            default: null,
        },
    },
    computed: {
        showedLabel() {
            return this.label !== null
                ? this.date.toLocaleDateString('es-ES', { year: 'numeric', month: 'long', day: '2-digit' })
                : this.totalLabel;
        },
        showedAmount() {
            return this.amount !== null
                ? this.amount
                : this.totalAmount;
        },
        showedAmountWithCurrency() {
            return currencyFormatter.format(this.showedAmount);
        },
    },
};
</script>

<style scoped>
main {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  width: 100%;
}

h1,
p {
  margin: 0;
  text-align: center;
}

.label {
    color: #444444;
    font-size: 2em;
    font-weight: bold;
}

h1 {
  margin-top: 14px;
  color: var(--brand-green);
}

.graphic {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  padding: 48px 24px;
  box-sizing: border-box;
}
</style>