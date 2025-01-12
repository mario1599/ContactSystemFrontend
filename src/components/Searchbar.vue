<template>
    <div class="search-bar">
      <input
        type="text"
        v-model="searchStore.query"
        placeholder="Otsi..."
      />
    </div>
  </template>
  
  <script lang="ts" setup>

  import { useSearchStore } from "@/store/search.ts";
  const searchStore = useSearchStore();


  </script>
  
  <style scoped>
  .search-bar {
    width: 100%;
    max-width: 25%;
    margin: 0 auto;
  }
  
  .search-bar input {
    width: 100%;
    padding: 10px;
    font-size: 16px;
    border-radius: 5px;
    border: 1px solid #ddd;
  }
  </style>
  