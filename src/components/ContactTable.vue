<template>
  <table class="table table-bordered mt-4">
    <thead>
      <tr>
        <th class="d-flex justify-content-between align-items-center">
          Nimi
          <i class="bi bi-arrow-down-up" style="cursor: pointer;" @click="sort('nimi')"></i>
        </th>
        <th class="justify-content-between align-items-center">
          Kood-nimi
          <i class="bi bi-arrow-down-up" style="cursor: pointer;" @click="sort('koodNimi')"></i>
        </th>
        <th>Telefoni number</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="contactItem in filteredContacts" :key="contactItem.id">
        <td>{{ contactItem.nimi }}</td>
        <td>{{ contactItem.koodNimi }}</td>
        <td>{{ contactItem.telefon }}</td>
      </tr>
      <tr v-if="contact.length === 0">
        <td colspan="3">No contacts available</td>
      </tr>
    </tbody>
  </table>
</template>

<script lang="ts" setup>
import { computed, onMounted, ref } from "vue";
import axios from "axios";
import { useSearchStore } from "@/store/search.ts";

const searchStore = useSearchStore();

const contact = ref([]);
const sortOrder = ref({ nimi: 'asc', koodNimi: 'asc' }); // Track sort order for each column

onMounted(async () => {
  try {
    const response = await axios.get('http://localhost:8080/api/v1/contact');
    contact.value = response.data;
  } catch (error) {
    console.error("Error fetching data:", error);
  }
});

// see sort mulle ei meeldi, mõtle ümber
function sort(column: 'nimi' | 'koodNimi') {
  contact.value.sort((a, b) => {
    return sortOrder.value[column] === 'asc'
      ? a[column].localeCompare(b[column])
      : b[column].localeCompare(a[column]);
  });
  sortOrder.value[column] = sortOrder.value[column] === 'asc' ? 'desc' : 'asc';
}


const filteredContacts = computed(() => {
  return contact.value.filter(contactItem => {
    return (
      contactItem.nimi.toLowerCase().includes(searchStore.query.toLowerCase()) ||
      contactItem.koodNimi.toLowerCase().includes(searchStore.query.toLowerCase())
    );
  });
});

</script>
