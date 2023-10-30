<template>
  <Layout>
    <template #header>
      <Header />
    </template>
    <template #resume>
      <Resume
        :date ="date"
        :total-amount="1000000"
        :amount="amount"
      >
        <template #graphic>
          graphic
        </template>
        <template #action>
          action
        </template>
      </Resume>
    </template>
    <template #movements>
      <Movements
        :movements="movements"
      />
    </template>
  </Layout>
</template>

<script>
import Layout from "@/components/Layout.vue";
import Header from "@/components/Header.vue";
import Resume from "@/components/Resume/Index.vue";
import Movements from "@/components/Movements/Index.vue";

export default {
  components: {
    Layout,
    Header,
    Resume,
    Movements,
  },
  data() {
    return {
      date: new Date(),
      amount: 22500,
      movements: [{
        id: 0,
        title: "Movimiento 1",
        description: "Descripción del movimiento 1",
        amount: 2500,
      }, {
        id: 1,
        title: "Movimiento 2",
        description: "Descripción del movimiento 2",
        amount: -2500,
      }, {
        id: 2,
        title: "Movimiento 3",
        description: "Descripción del movimiento 3",
        amount: 2500,
      }, {
        id: 3,
        title: "Movimiento 4",
        description: "Descripción del movimiento 4",
        amount: -2500,
      }, {
        id: 4,
        title: "Movimiento 5",
        description: "Descripción del movimiento 5",
        amount: 2500,
      }, {
        id: 5,
        title: "Movimiento 6",
        description: "Descripción del movimiento 6",
        amount: -2500,
      }, {
        id: 6,
        title: "Movimiento 7",
        description: "Descripción del movimiento 7",
        amount: 2500,
      }, {
        id: 7,
        title: "Movimiento 8",
        description: "Descripción del movimiento 8",
        amount: -2500,
      },
       {
        id: 8,
        title: "Movimiento 9",
        description: "Descripción del movimiento 9",
        amount: 2500,
      }]
    }
  },
};
</script>