<template>
  <div>
    <UVerticalNavigation :links="links" />
    <router-view/>
  </div>
</template>

<script setup lang="ts">
import Product from "~/pages/Product.vue";
import StudentList from "~/pages/StudentList.vue";

const links = ref([
  [{
    label: 'StudentList',
    icon: 'i-heroicons-home',
    to: '/StudentList'
  }, {
    label: 'Product',
    icon: 'i-heroicons-chart-bar',
    to: '/Product'
  }]
])
</script>