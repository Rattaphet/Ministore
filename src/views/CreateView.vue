<template>
    <div class="q-gutter-md" style="max-width: 500px; margin: auto; padding: 20px;">
      <q-card class="q-pa-md shadow-2 rounded-borders">
        <q-form @submit.prevent="onSubmit">
          <q-input v-model="first_name" label="First Name" outlined class="q-mb-md" />
          <q-input v-model="last_name" label="Last Name" outlined class="q-mb-md" />
          <q-input v-model="email" label="Email" outlined type="email" class="q-mb-md" />
          <q-input v-model="address" label="Address" outlined class="q-mb-md" />
          <q-input v-model="phone_number" label="Phone Number" outlined type="tel" class="q-mb-md" />
  
          <div class="text-center q-mt-md">
            <q-btn type="submit" label="Submit" color="primary" class="rounded-btn q-mb-md" />
          </div>
        </q-form>
      </q-card>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue'
  import { useRouter } from 'vue-router'
  
  const router = useRouter()
  
  const first_name = ref('')
  const last_name = ref('')
  const email = ref('')
  const address = ref('')
  const phone_number = ref('')
  
  const onSubmit = async () => {
    try {
      const response = await fetch("http://localhost:8800/api/v1/customers", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
          first_name: first_name.value,
          last_name: last_name.value,
          address: address.value,
          email: email.value,
          phone_number: phone_number.value
        })
      })
      const result = await response.json()
  
      if (result.status === 'ok') {
        alert("เพิ่มข้อมูลสำเร็จ!")
        router.push('/')
      } else {
        alert("เกิดข้อผิดพลาด: " + result.message)
      }
    } catch (error) {
      console.error("Error:", error)
      alert("เกิดข้อผิดพลาดในการส่งข้อมูล")
    }
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
  