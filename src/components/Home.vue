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
          <Graphic :amounts="latestAmounts" />
        </template>
        <template #action>
          <Action />
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
import Action from "@/components/Action.vue";
import Graphic from "./Resume/Graphic.vue";

export default {
  components: {
    Layout,
    Header,
    Resume,
    Movements,
    Action,
    Graphic,
  },
  data() {
    return {
      date: new Date(),
      amount: 22500,
      movements: [{
        id: 0,
        title: "Movimiento 1",
        description: "Descripción del movimiento 1",
        amount: 100,
        movementDate: new Date("09-25-2023"),
      }, {
        id: 1,
        title: "Movimiento 2",
        description: "Descripción del movimiento 2",
        amount: 200,
        movementDate: new Date("10-06-2023"),
      }, {
        id: 2,
        title: "Movimiento 3",
        description: "Descripción del movimiento 3",
        amount: 500,
        movementDate: new Date("10-15-2023"),
      }, {
        id: 3,
        title: "Movimiento 4",
        description: "Descripción del movimiento 4",
        amount: 200,
        movementDate: new Date("10-20-2023"),
      }, {
        id: 4,
        title: "Movimiento 5",
        description: "Descripción del movimiento 5",
        amount: -400,
        movementDate: new Date("10-24-2023"),
      }, {
        id: 5,
        title: "Movimiento 6",
        description: "Descripción del movimiento 6",
        amount: -600,
        movementDate: new Date("10-30-2023"),
      }, {
        id: 6,
        title: "Movimiento 7",
        description: "Descripción del movimiento 7",
        amount: -300,
        movementDate: new Date("11-01-2023"),
      }, {
        id: 7,
        title: "Movimiento 8",
        description: "Descripción del movimiento 8",
        amount: 0,
        movementDate: new Date("11-03-2023"),
      }, {
        id: 8,
        title: "Movimiento 9",
        description: "Descripción del movimiento 9",
        amount: 300,
        movementDate: new Date("11-05-2023"),
      }, {
        id: 9,
        title: "Movimiento 9",
        description: "Descripción del movimiento 9",
        amount: 500,
        movementDate: new Date("11-06-2023"),
      }]
    }
  },
  computed: {
    latestAmounts() {
      return this.movements
        .filter(movement => {
          const today = new Date();
          const limitDate = today.setDate(today.getDate() - 30);

          return movement.movementDate > limitDate;
        })
        .map(movement => movement.amount);
    },
    totalLatestAmounts() {
      return this.latestAmounts.reduce(
        (sum, amount) => sum + amount,
        0
      );
    },
  }
};
</script>