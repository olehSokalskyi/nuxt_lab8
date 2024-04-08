<script setup lang="ts">
const links = [{
  label: 'StudentList',
  to: 'StudentList'
}, {
  label: 'Product',
  to: 'Product'
}]
</script>

<template>
  <UVerticalNavigation :links="links">
    <template #default="{ link }">
      <span class="group-hover:text-primary relative">{{ link.label }}</span>
    </template>
  </UVerticalNavigation>
  <NuxtPage/>
</template>

