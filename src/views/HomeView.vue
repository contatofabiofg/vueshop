<template>
  <div class="home">
    <ProductList />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import ProductList from '@/components/ProductList.vue'; // @ is an alias to /src

export default defineComponent({
  name: 'HomeView',
  components: {
    ProductList,
  },
});
</script>
