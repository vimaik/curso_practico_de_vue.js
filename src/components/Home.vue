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
      <Movements />
    </template>
  </Layout>
</template>

<script>
import Layout from "@/components/Layout.vue";
import Header from "@/components/Header.vue";
import Resume from "@/components/Resume/Index.vue";
import Movements from "@/components/Movements.vue";

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
      // date: null,
      amount: 1000,
      // amount: null,
    }
  },
};
</script>