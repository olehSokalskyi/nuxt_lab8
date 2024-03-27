<template>
  <h1 class="text-center text-3xl my-[2%]">Product List</h1>
  <div class="mx-[3%] my-[2%]">
    <div class="flex justify-between items-center w-full px-4 py-3">
    </div>
    <div class="flex px-3 py-3.5 border-b border-gray-200 dark:border-gray-700">
      <UInput
          v-model="q"
          placeholder="Filter people..."
      />
    </div>
    <div class="flex justify-end px-3 py-3.5 border-t border-gray-200 dark:border-gray-700">
      <UPagination
          v-model="page"
          :page-count="pageCount"
          :total="filteredRows.length"
      />
    </div>
    <UTable :rows="rows"
            :columns="columns"
            :loading="pending"
            :loading-state="{ icon: 'i-heroicons-arrow-path-20-solid', label: 'Loading...' }"
            :progress="{ color: 'primary', animation: 'carousel' }"
    >
      <template #rating-data="{ row }">
        <span
            :style="{ color: row.rating > 4.5 ? 'green' : 'red' }">
            {{ row.rating }}
        </span>
      </template>
      <template #thumbnail-data="{ row }">
        <img class="w-[100px] h-[100px]" :src="row.thumbnail" alt="Thumbnail" />
      </template>
    </UTable>
  </div>
</template>

<script setup lang="ts">
const { pending,  data } = await useLazyAsyncData<any>('products',()=> $fetch('https://dummyjson.com/products') as any);

const products = data.value.products;

const columns = [{
  key: 'id',
  label: 'ID',
  sortable: true
}, {
  key: 'title',
  label: 'Title',
  sortable: true

}, {
  key: 'description',
  label: 'Description',
  sortable: true
},{
  key:'rating',
  label: 'Rating',
  sortable: true
}, {
  key: 'price',
  label: 'Price',
  sortable: true

}, {
  key: 'brand',
  label: 'Brand',
  sortable: true

},{
  key: 'category',
  label: 'Category',
  sortable: true

},{
  key:"thumbnail",
  label: "Thumbnail"
}]


const q = ref('')
const page = ref(1)
const pageCount = 5

const filteredRows = computed(() => {
  return products.filter((product: any) => {
    return Object.values(product).some((value) => {
      return String(value).toLowerCase().includes(q.value.toLowerCase())
    })
  })
})

const rows = computed(() => {
  if (!q.value) {
    return products.slice((page.value - 1) * pageCount, (page.value) * pageCount)
  }

  return products.filter((person: any) => {
    return Object.values(person).some((value) => {
      return String(value).toLowerCase().includes(q.value.toLowerCase())
    })
  })
})
</script>
