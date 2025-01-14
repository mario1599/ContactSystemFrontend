<template>
  <div class="container justify-content-center align-items-center vh-100 mt-4">
    <div class="flex flex-row d-flex mb-3">
    <Searchbar />
    <NewContact />
  </div>
     <ContactTable/>
    </div>
</template>


<script setup>
import NewContact from '@/components/NewContact.vue'
import Searchbar from '@/components/Searchbar.vue'
import ContactTable from '@/components/ContactTable.vue'
</script>

