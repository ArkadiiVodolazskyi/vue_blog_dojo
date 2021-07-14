<template>
  <div class="home">
    <h2>Search from names</h2>
    <input type="text" v-model="searchValue">
    <ul>
      <li v-for="name in searchNames" :key="name">
        {{ name }}
      </li>
    </ul>
  </div>
</template>

<script>
import { ref } from '@vue/reactivity';
import { computed } from '@vue/runtime-core';

export default {
  name: "Home",
  setup() {

    const searchValue = ref('');
    const names = ref(['Arkadii', 'Luda', 'Alexey', 'Alla', 'Rita', 'Veniamin']);

    const searchNames = computed(() => {
      return names.value.filter((name) => {
        return name.includes(searchValue.value);
      })
    });

    return {
      searchValue,
      searchNames
    }
  }
};
</script>
