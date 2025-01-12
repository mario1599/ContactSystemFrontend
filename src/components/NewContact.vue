<template>
    <div class="button-container">
      <button @click="toggleDiv" class="btn btn-primary">
        <i class="bi bi-person-plus-fill"></i> Lisa kontakt
      </button>
  
      <!-- Hidden form container -->
      <div v-if="isDivVisible" class="overlay">
        <div class="form-box p-4">
          <h3 class="text-center mb-3">Lisa kontakt</h3>
          <!-- Form with three input fields -->
          <form @submit.prevent="submitForm">
            <div class="mb-3">
              <label for="nimi" >Nimi:</label>
              <input v-model="formData.nimi" type="text" id="nimi" class="form-control" required />
            </div>
  
            <div class="mb-3">
              <label for="koodnimi">Kood-nimi:</label>
              <input v-model="formData.koodnimi" type="text" id="koodnimi" class="form-control" required />
            </div>
  
            <div class="mb-3">
              <label for="tel" class="form-label">Telefoni number</label>
              <input v-model="formData.telefoniNumber" type="tel" id="tel" class="form-control" required />
            </div>
            <div class="d-flex justify-content-between">
              <button type="submit" class="btn btn-success">Lisa</button>
              <button @click="toggleDiv" type="button" class="btn btn-danger">Tagasi</button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </template>
  
  <script lang="ts" setup>
  import { ref } from 'vue';
  import axios from "axios";
  
  const isDivVisible = ref(false);
  const formData = ref({
    nimi: "",
    koodnimi: "",
    telefoniNumber: ""
  });
  
  const toggleDiv = () => {
    isDivVisible.value = !isDivVisible.value;
  };
  
  const submitForm = async () => {
    try {
      
    const params = new URLSearchParams();
    params.append('nimi', formData.value.nimi);
    params.append('koodNimi', formData.value.koodnimi);
    params.append('telefon', formData.value.telefoniNumber);
      
    const url = `http://localhost:8080/api/v1/contact/add-contact`;
    await axios.post(url, params);

    //lisada check telefoninumbri jaoks ja character limit ka 
    formData.value.nimi = "";
    formData.value.koodnimi = "";
    formData.value.telefoniNumber = "";
    window.location.reload();
} catch (error) {
    console.error('Error adding contact:', error);
    alert('There was an error adding the contact.');
  }

  };
  </script>
  
  <style scoped>
  .overlay {
    position: fixed;
    top: 0;
    left: 0;
    height: 100vh;
    width: 100vw;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 1000;
  }
  
  .form-box {
    background: white;
    border-radius: 10px;
    box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
    max-width: 400px;
    width: 100%;
  }
  </style>
  