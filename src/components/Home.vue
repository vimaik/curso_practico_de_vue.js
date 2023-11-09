<template>
  <Layout>
    <template #header>
      <Header />
    </template>
    <template #resume>
      <Resume
        :total-label="'Ahorro total'"
        :label="label"
        :total-amount="totalAmount"
        :amount="amount"
      >
        <template #graphic>
          <Graphic :amounts="latestAmounts" />
        </template>
        <template #action>
          <Action
            @create="create"
            :new-movement-id="newMovementId"
          />
        </template>
      </Resume>
    </template>
    <template #movements>
      <Movements
        :movements="movements"
        @remove="remove"
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
      label : null,
      amount: null,
      movements: [],
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
    newMovementId() {
      if(this.movements.length > 0) {
        return Math.max(...this.movements.map(m => m.id)) + 1;
      } else {
        return 0;
      }
    },
    totalAmount() {
      return this.movements.reduce((total, movement) => total + movement.amount, 0);
    },
  },
  mounted() {
    const movements = JSON.parse(localStorage.getItem("movements"));

    if(Array.isArray(movements))  {
      this.movements = movements.map(m => {
        return { ...m, movementDate: new Date(m.movementDate) };
      });
  }
  },
  methods: {
    create(movement) {
      this.movements.push(movement);
      this.save();
    },
    remove(id) {
      const index = this.movements.findIndex(m => m.id === id);
      this.movements.splice(index, 1);
      this.save();
    },
    save() {
      localStorage.setItem("movements", JSON.stringify(this.movements));
    }
  }
};
</script>