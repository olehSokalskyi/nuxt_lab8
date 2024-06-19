<template>
  <div>
    <div v-if="post">
      <div class="flex center mb-4">
      </div>
    <UForm :schema="schema" :state="state" @submit='submitPost'>
    <div v-if="activeTab === 'Details'">
        <div class="flex justify-center">
          <div class="w-full md:w-2/3">
            <div class="bg-gray-800 shadow-md rounded-lg p-4">
              <h3 class="text-xl font-bold mb-4">Create Post</h3>
              <button
                  v-for="tab in tabs"
                  :key="tab"
                  :class="['px-3 py-1 mr-2 rounded focus:outline-none', activeTab === tab ? 'bg-blue-500 text-white' : 'bg-gray-200']"
                  @click="activeTab = tab"
              >
                {{ tab }}
              </button>
              <UFormGroup label="Title" name="title">
                <UInput class="w-full" id="title" v-model="state.title"/>
              </UFormGroup>
              <UFormGroup label="Category" name="category">
                <select class="w-full p-2 bg-gray-900" id="category" v-model="post.category">
                  <option v-for="category in categories" :key="category.id" :value="category">
                    {{ category.title }}
                  </option>
                </select>
              </UFormGroup>
              <UFormGroup label="Post Content" name="content">
                <UTextarea class="w-full " id="content" v-model="state.content_raw" :rows="5"/>
              </UFormGroup>
              <UButton type="submit">Відправити</UButton>
            </div>
          </div>
        </div>
      </div>

      <div v-if="activeTab === 'Content'">
        <div class="flex justify-center">
          <div class="w-full md:w-2/3">
            <div class="bg-gray-800 shadow-md rounded-lg p-4">
              <h3 class="text-xl font-bold mb-4">More detail</h3>
              <button
                  v-for="tab in tabs"
                  :key="tab"
                  :class="['px-3 py-1 mr-2 rounded focus:outline-none', activeTab === tab ? 'bg-blue-500 text-white' : 'bg-gray-200']"
                  @click="activeTab = tab"
              >
                {{ tab }}
              </button>
              <UFormGroup label="Excerpt" name="excerpt">
                <UInput class="w-full" id="excerpt" v-model="state.excerpt"/>
              </UFormGroup>
              <UFormGroup label="Publish" name="is_published">
                <input type="checkbox" id="is_published" v-model="state.is_published"/>
              </UFormGroup>
              <UFormGroup label="Slug" name="slug">
                <UInput class="w-full" id="slug" v-model="state.slug"/>
              </UFormGroup>
            </div>
          </div>
        </div>
      </div>

    </UForm>
    </div>

  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { z } from 'zod'
import { useRouter } from 'vue-router';
import { API_SERVER_URL } from '../../untils/constants';

interface Post {
  id?: number;
  title: string;
  content_raw: string;
  published_at?: string;
  category?: Category;
  is_published: boolean;
  excerpt: string;
  slug: string;
  category_id?: number | undefined;
}

interface Category {
  id: number;
  title: string;
}
const state = reactive({
  title: '',
  content_raw: '',
  category: { id : 0, title: '' },
  is_published: false,
  excerpt: '',
  slug: '',
  category_id : 0
})

const schema = z.object({
  title: z.string().min(5,'Must be at least 5 characters'),
  content_raw: z.string().min(5,'Must be at least 5 characters'),
  category_id: z.number(),
  is_published: z.boolean(),
  excerpt: z.string(),
  slug: z.string()
})
const post = ref<Post>({
  title: '',
  content_raw: '',
  category: { id : 0, title: '' },
  is_published: false,
  excerpt: '',
  slug: ''
});
const tabs = ref(['Details', 'Content']);
const activeTab = ref('Details');
const router = useRouter();
const categories = ref<Category[]>([]);

const getCategories = async () => {
  try {
    const response = await $fetch<Category[]>(`${API_SERVER_URL}/api/blog/categories/forCombobox`);
    categories.value = response;
  } catch (error) {
    console.error('Failed to fetch categories:', error);
  }
};

onMounted(() => {
  getCategories();
});

const submitPost = async () => {
  try {
    state.category_id = post.value.category?.id;
    console.log(state)
    await $fetch(`${API_SERVER_URL}/api/blog/posts/create`, {
      method: 'POST',
      body:  state
    });
    router.push('/posts/'); // Redirect after successful submission
  } catch (error) {
    console.error('Failed to submit post:', error);
  }
};
</script>