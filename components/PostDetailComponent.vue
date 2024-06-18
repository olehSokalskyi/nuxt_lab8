

<template>
  <div class="row justify-content-center">
    <div class="col-md-12">
      <div class="card">
        <div class="card-header">
          <h3 class="card-title">Post Detail</h3>
          <div v-if="post && post.is_published">
            Опубліковано
          </div>
          <div v-else>
            Чернетка
          </div>
          <div class="card-body">
            <div class="card-title"></div>
            <div class="card-subtitle mb-2 text-muted"></div>
            <div class="form-group">
              <label for="title">Title</label>
              <input type="text" class="form-control" id="title" v-model="post.title" readonly>
            </div>
            <div class="form-group">
              <label for="user">User</label>
              <input type="text" class="form-control" id="user" v-model="post.user.name" readonly>
            </div>

          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { useRoute } from 'vue-router'

const route = useRoute()
const id = Number(route.params.id)
interface Post {
  id: number;
  title: string;
  published_at: string;
  user: User,
  category: Category,
  is_published: boolean;
}
interface User{
  name: string;
}
interface Category{
  title: string;
}

const post = ref<Post>();

const getPost = async (id: number) =>{
  $fetch<Post>(`http://localhost:8000/api/blog/posts/${id}`)
      .then(response => {
        console.log(response);
        post.value = response;
      })
}
onMounted(() => {
  getPost(id)
})
</script>

<style scoped>

</style>