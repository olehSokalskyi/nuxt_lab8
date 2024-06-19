<template>
  <div class="flex flex-col">
    <div class="me-5">
      <NuxtLink to="/posts/create">
        <button class="bg-transparent hover:bg-blue-500 float-end text-blue-700 font-semibold hover:text-white py-2 px-4 border border-blue-500 hover:border-transparent rounded">
          Add Post
        </button>
      </NuxtLink>
    </div>
    <div>
      <UTable class="mt-3" :columns="columns" :rows="rows" :total="totalPosts">
        <template #actions-data="{ row }">
          <UDropdown :items="items(row)">
            <UButton color="gray" variant="ghost" icon="i-heroicons-ellipsis-horizontal-20-solid" />
          </UDropdown>
        </template>
      </UTable>
      <div class="d-flex mt-5">
        <UPagination v-model="page" :page-count="5" :total="totalPosts" />
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import axios from 'axios';
import { ref, computed } from 'vue';
import { API_SERVER_URL } from '~/untils/constants';

interface Post {
  id: number;
  title: string;
  published_at: string;
  user: User;
  category: Category;
}

interface User {
  id:number;
  name: string;
}

interface Category {
  id:number;
  title: string;
}

const columns = [
  { key: 'id', label: 'ID' },
  { key: 'title', label: 'Title' },
  { key: 'published_at', label: 'Published At' },
  { key: 'user', label: 'User' },
  { key: 'category', label: 'Category' },
  { key: 'actions', label: 'Actions' }
];

const page = ref(1);
const totalPages = ref(1);
const posts = ref<Post[]>([]);
const totalPosts = ref(0);

const getPosts = async (page= 1) => {
  try {
    const response = await axios.get(`${API_SERVER_URL}/api/blog/posts?page=${page}`);
    posts.value = response.data.data;
    totalPages.value = response.data.last_page;
    totalPosts.value = response.data.total;
    console.log(response)
  } catch (error) {
    console.error('Error fetching posts:', error);
  }
};

watch(page, () => {
  getPosts(page.value);
});
getPosts(page.value);
const rows = computed(() => {
  return posts.value.map(post => ({
    id: post.id,
    title: post.title,
    published_at: post.published_at,
    user: post.user.name,
    category: post.category.title
  }));
});

const items = (post: Post) => [
  [
    {
      label: 'Edit',
      icon: 'i-heroicons-pencil-square-20-solid',
      click: () => window.location.href = (`/posts/edit/${post.id}`)
    },
    {
      label: 'Details',
      icon: 'i-heroicons-information-circle-20-solid',
      click: () => window.location.href = (`/posts/${post.id}`)
    }
  ],
  [
    {
      label: 'Delete',
      icon: 'i-heroicons-trash-20-solid',
      click: () => {
        if (confirm(`Are you sure you want to delete post '${post.title}'?`)) {
          axios.delete(`${API_SERVER_URL}/api/blog/posts/delete/${post.id}`)
              .then(res => {
                console.log(res);
                alert('Post deleted!');
                getPosts(page.value);
              })
              .catch(error => {
                console.error(error);
                alert('Failed to delete posts!');
              });
        }
      }
    }
  ]
];
</script>
