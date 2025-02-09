<template>
    <div class="q-gutter-md" style="max-width: 500px; margin: auto; padding: 20px;">
      <q-card class="q-pa-md shadow-2 rounded-borders">
        <q-form @submit.prevent="onSubmit">
          <!-- ID (readonly) -->
          <q-input v-model="id" label="ID" readonly class="q-mb-md" />
  
          <!-- Editable Fields -->
          <q-input v-model="first_name" label="First Name" class="q-mb-md" />
          <q-input v-model="last_name" label="Last Name" class="q-mb-md" />
          <q-input v-model="email" label="Email" class="q-mb-md" />
          <q-input v-model="address" label="Address" class="q-mb-md" />
          <q-input v-model="phone_number" label="Phone Number" class="q-mb-md" />
  
          <!-- Submit Button -->
          <div class="text-center q-mt-md">
            <q-btn type="submit" label="Update" color="primary" class="rounded-btn q-mb-md" />
          </div>
        </q-form>
      </q-card>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue'
  import { useRouter, useRoute } from 'vue-router'
  
  const router = useRouter()
  const route = useRoute()
  
  // Reactive variables for form fields
  const id = ref(route.params.id)
  const first_name = ref('')
  const last_name = ref('')
  const email = ref('')
  const address = ref('')
  const phone_number = ref('')
  
  // Fetch existing customer data
  const fetchData = async () => {
    fetch(`http://localhost:8800/api/v1/customers/${id.value}`)
      .then(res => res.json())
      .then(result => {
        first_name.value = result.first_name
        last_name.value = result.last_name
        email.value = result.email
        address.value = result.address
        phone_number.value = result.phone_number
      })
  }
  fetchData()
  
  // Handle form submission and update customer data
  const onSubmit = async () => {
    const myHeaders = new Headers()
    myHeaders.append('Content-Type', 'application/json')
  
    const raw = JSON.stringify({
      first_name: first_name.value,
      last_name: last_name.value,
      email: email.value,
      address: address.value,
      phone_number: phone_number.value,
    })
  
    const requestOptions = {
      method: 'PUT',
      headers: myHeaders,
      body: raw,
      redirect: 'follow',
    }
  
    fetch(`http://localhost:8800/api/v1/customers/${id.value}`, requestOptions)
      .then((response) => {
        if (!response.ok) {
          return response.json().then((error) => {
            throw new Error(error.message || 'Something went wrong.')
          })
        }
        return response.json()
      })
      .then((result) => {
        alert(result.message)
        if (result.status === 'ok') {
          router.push('/')
          fetchData() // Refresh data after update
        }
      })
      .catch((error) => {
        console.error(error)
        alert(`Error: ${error.message}`)
      })
  }
  </script>
  
  <style scoped>
  .q-card {
    border-radius: 10px;
  }
  
  .q-btn {
    font-size: 16px;
    padding: 10px 20px;
  }
  
  .q-input {
    font-size: 14px;
  }
  
  .rounded-btn {
    border-radius: 25px;
  }
  
  .text-center {
    text-align: center;
  }
  
  .q-pa-md {
    padding: 20px;
  }
  </style>
  