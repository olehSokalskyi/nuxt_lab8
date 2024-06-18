<script setup lang="ts">
import { useRoute } from 'vue-router'

const route = useRoute()
const id = Number(route.params.id)

const links = [{
  label: 'StudentList',
  to: '/StudentList'
},{
  label: 'BlogPostsUI',
  to: '/BlogPostsUI'
}]
</script>
<template>
  <UHorizontalNavigation :links="links">
    <template #default="{ link }">
      <span class="group-hover:text-primary relative">{{link.label}}</span>
    </template>
  </UHorizontalNavigation>
  <router-view/>
</template>

<style>

</style>
