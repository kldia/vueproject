<script setup>
import { ref, reactive } from 'vue'
import Header from './components/Header.vue'
import Sidebar from './components/Sidebar.vue'
import Table from './components/Table.vue'
import EditableTable from './components/EditableTable.vue'

const sidebarCollapsed = ref(false)

const items = ref([
  { id: 1, name: 'Order A', createdAt: '2025-07-20 12:00', createdBy: 'Nino', type: 'Retail', status: 'New' },
  { id: 2, name: 'Order B', createdAt: '2025-07-21 09:30', createdBy: 'Giorgi', type: 'Wholesale', status: 'Processing' },
  { id: 3, name: 'Order C', createdAt: '2025-07-22 17:15', createdBy: 'Ana', type: 'Retail', status: 'Done' }
])

const activeItem = ref(null)
let editableRows = ref([])

function openDetails(item) {
  activeItem.value = item
  // სატესტო მონაცემები
  editableRows.value = [
    { id: 1, name: 'Item 1', qty: 2, price: 10.5, unit: 'pcs', comment: '' },
    { id: 2, name: 'Item 2', qty: 1, price: 5, unit: 'pcs', comment: '' }
  ]
}

function closeDetails() {
  activeItem.value = null
  editableRows.value = []
}

function saveRows(rows) {
  // სერვერისთვის გაგზავნა ინფორმაციის მოხდება აქ
  console.log('Saving rows for', activeItem.value, rows)

  alert('Rows saved (open console for data)')
}

function editItem(item) {
  alert('Edit item: ' + item.name)
}

function deleteItem(item) {
  if (confirm('Delete ' + item.name + '?')) {
    items.value = items.value.filter(i => i.id !== item.id)
  }
}

function sendItem(item) {
  alert('Send: ' + item.name)
}

function onLogout() {
  alert('Logout clicked')
}

</script>

<template>
  <div class="app-root">
    <Header @logout="onLogout" />
    <div class="content">
      <Sidebar :collapsed="sidebarCollapsed" @toggle="sidebarCollapsed = !sidebarCollapsed" />

      <main class="main-area">
        <section class="list-section">
          <Table
            :items="items"
            @view-details="openDetails"
            @edit-item="editItem"
            @delete-item="deleteItem"
            @send-item="sendItem"
          />
        </section>

        <section class="details-section" v-if="activeItem">
          <h3>Editing: {{ activeItem.name }}</h3>
          <EditableTable
            :rows="editableRows"
            @update:rows="val => (editableRows = val)"
            @close="closeDetails"
            @save="saveRows"
          />
        </section>
      </main>
    </div>
  </div>
</template>


