<template>
  <h1 class="text-center text-3xl my-[2%]">Student List</h1>
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
            :loading-state="{ icon: 'i-heroicons-arrow-path-20-solid', label: 'Loading...' }"
            :progress="{ color: 'primary', animation: 'carousel' }"
    />
  </div>
</template>

<script setup lang="ts">
useHead({
  title: 'Student List'
})

const StudentList = [
  {id: 1, FName: 'John', SName: 'Doe', Group: 'A'},
  {id: 2, FName: 'Jane', SName: 'Doe', Group: 'B'},
  {id: 3, FName: 'John', SName: 'Smith', Group: 'A'},
  {id: 4, FName: 'John', SName: 'Doe', Group: 'A'},
  {id: 5, FName: 'Jane', SName: 'Doe', Group: 'B'},
  {id: 6, FName: 'John', SName: 'Smith', Group: 'A'},
  {id: 7, FName: 'John', SName: 'Doe', Group: 'A'},
  {id: 8, FName: 'Jane', SName: 'Doe', Group: 'B'},
  {id: 9, FName: 'John', SName: 'Smith', Group: 'A'},

]

const columns = [{
  key: 'id',
  label: 'ID',
  sortable: true
}, {
  key: 'FName',
  label: 'FName',
  sortable: true

}, {
  key: 'SName',
  label: 'FName',
  sortable: true
},{
  key:'Group',
  label: 'Group',
  sortable: true
}]
const q = ref('')
const page = ref(1)
const pageCount = 5

const filteredRows = computed(() => {
  return StudentList.filter((product: any) => {
    return Object.values(product).some((value) => {
      return String(value).toLowerCase().includes(q.value.toLowerCase())
    })
  })
})

const rows = computed(() => {
  if (!q.value) {
    return StudentList.slice((page.value - 1) * pageCount, (page.value) * pageCount)
  }

  return StudentList.filter((person: any) => {
    return Object.values(person).some((value) => {
      return String(value).toLowerCase().includes(q.value.toLowerCase())
    })
  })
})
</script>
