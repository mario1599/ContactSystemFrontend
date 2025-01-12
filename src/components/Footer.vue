<template>
  <div class="border-top py-3 d-flex justify-content-between align-items-center">

  </div>
</template>
  
<script lang="ts" setup>
import { ref, type Ref } from "vue";
import Refs from "@/components/Refs.vue";

const themeToggler: Ref<boolean> = ref(false);
const docElm = document.documentElement;

const changeTheme = (e: Event) => {
  e.preventDefault();

  themeToggler.value = !themeToggler.value;
  docElm.dataset.bsTheme = themeToggler.value ? "dark" : "light";
};
</script>