<template>
  <table class="table table-bordered mt-4">
    <thead>
      <tr>
        <th class=" justify-content-between align-items-center">
          Nimi
          <i class="bi bi-arrow-down-up" style="cursor: pointer;" @click="sort('nimi')"></i>
        </th>
        <th class="justify-content-between align-items-center">
          Kood-nimi
          <i class="bi bi-arrow-down-up" style="cursor: pointer;" @click="sort('koodNimi')"></i>
        </th>
        <th>Telefoni number
          <i class="bi bi-arrow-down-up" style="cursor: pointer;" @click="sort('telefon')"></i>
        </th style="max-width:10px">
        <th>
        </th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="contactItem in filteredContacts" :key="contactItem.id">
        <td>{{ contactItem.nimi }}</td>
        <td>{{ contactItem.koodNimi }}</td>
        <td>{{ contactItem.telefon }}</td>
        <td><i class="bi bi-trash" style="cursor: pointer;" @click="removeContact(contactItem.id)"></i></td>
      </tr>
      <tr v-if="contact.length === 0">
        <td colspan="3">Kontaktid puuduvad</td>
      </tr>
    </tbody>
  </table>
</template>

<script setup>
import { computed, onMounted, ref } from "vue";
import axios from "axios";
import { useSearchStore } from "@/store/search";

const searchStore = useSearchStore();

const contact = ref([]);
const sortOrder = ref({ nimi: 'asc', koodNimi: 'asc', telefon: 'asc'});

onMounted(async () => {
  try {
    const response = await axios.get('http://localhost:8080/api/v1/contact');
    contact.value = response.data;
  } catch (error) {
    console.error("Error fetching data:", error);
  }
});


function sort(column) {
  const isAscending = sortOrder.value[column] === 'asc';
  
  sortOrder.value[column] = isAscending ? 'desc' : 'asc';

  contact.value.sort((a, b) => {
    const compareResult = a[column].localeCompare(b[column]);
    return isAscending ? compareResult : -compareResult;
  });
}

const filteredContacts = computed(() => {
  return contact.value.filter(contactItem => {
    return (
      contactItem.nimi.toLowerCase().includes(searchStore.query.toLowerCase()) ||
      contactItem.koodNimi.toLowerCase().includes(searchStore.query.toLowerCase()) ||
      contactItem.telefon.toLowerCase().includes(searchStore.query.toLowerCase())
    );
  });
});

function removeContact(id) {
  axios.delete(`http://localhost:8080/api/v1/contact/delete-contact?id=${id}`)
  .then(response => {
    console.log(response.data)
    console.log(`Deleted post with ID ${id}`);
    window.location.reload()
  })
  .catch(error => {
    console.error(error);
  });
}

</script>
