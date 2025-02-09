<template>
  <div class="q-pa-md">
    <!-- Create Button Section -->
    <div class="q-pa-md">
      <q-btn label="Create New Customer" color="primary" icon="add" @click="OnCreate" class="q-mb-md" />
    </div>

    <!-- Customers Table -->
    <q-table
      title="Customer List"
      :rows="rows"
      :columns="columns"
      row-key="customer_id"
      class="q-mb-md"
      :rows-per-page-options="[5, 10, 20, 50]"
    >
      <template v-slot:body-cell-actions="props">
        <q-td :props="props">
          <!-- Edit Button -->
          <q-btn flat dense round icon="edit" color="primary" @click="onEdit(props.row.customer_id)" class="q-mr-sm" />
          <!-- Delete Button -->
          <q-btn flat dense round icon="delete" color="negative" @click="onDelete(props.row.customer_id)" />
        </q-td>
      </template>
    </q-table>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import router from '../router'

// Column definition for the table
const columns = [
  { name: 'customer_id', align: 'center', label: 'Customer ID', field: 'customer_id', sortable: true },
  { name: 'first_name', align: 'center', label: 'First Name', field: 'first_name', sortable: true },
  { name: 'last_name', align: 'center', label: 'Last Name', field: 'last_name', sortable: true },
  { name: 'email', align: 'center', label: 'Email', field: 'email', sortable: true },
  { name: 'address', align: 'center', label: 'Address', field: 'address', sortable: true },
  { name: 'phone_number', align: 'center', label: 'Phone Number', field: 'phone_number', sortable: true },
  { name: 'actions', align: 'center', label: 'Actions', field: 'id', sortable: false }
]

// Data fetching
const rows = ref([])

const fetchData = async () => {
  fetch('http://localhost:8800/api/v1/customers')
    .then(res => res.json())
    .then(result => {
      rows.value = result
    })
}
fetchData()

// Navigate to create page
const OnCreate = () => {
  router.push('/create')
}

// Navigate to edit page
const onEdit = (id) => {
  router.push('update/' + id)
}

// Handle delete action
const onDelete = (id) => {
  const myHeaders = new Headers();
  myHeaders.append("Content-Type", "application/json");
  const requestOptions = {
    method: "DELETE",
    headers: myHeaders,
    redirect: "follow"
  };

  fetch(`http://localhost:8800/api/v1/customers/${id}`, requestOptions)
    .then((response) => response.json())
    .then((result) => {
      alert(result.message)
      if (result.status === "ok") {
        fetchData() // Refresh data after deletion
      }
    })
    .catch((error) => console.error(error));
}
</script>

<style scoped>
/* Additional styles for spacing and button design */
.q-table {
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}
.q-btn {
  font-size: 14px;
}
</style>
