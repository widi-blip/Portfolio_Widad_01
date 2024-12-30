<template>
  <div class="">
    <ul 
      v-for="post in folders"
      :key="post.slug"
      >
      ggg
      <NuxtLink :to="post._path" >
          <li class="">{{ post.title }}j</li>
      </NuxtLink>
    </ul>
    <hr>
  </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue';
  import { useAsyncData } from '#app';
  
  // Define a ref for storing folders
  const folders = ref([]);
  
  // Fetch all posts from both /folders and /page
  const { data: foldersPosts } = await useAsyncData('folders', () => queryContent('/')
  .sort({ numbernavigation: 1 })
  .find());

  // Combine and filter the data
  onMounted(() => {
    const allPosts = [...(foldersPosts.value || [])];
    if (allPosts.length) {
      folders.value = allPosts.filter(post => post.included && post.included === true && post.included !== false);
    }
  });
  </script>
  
<style>
  hr{
    opacity: .1;
  }

  li{
    font-family: "Space Mono", monospace;
  }
</style>
