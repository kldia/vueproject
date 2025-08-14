<template>
  <div class="table-wrapper">
    <table class="main-table">
      <thead>
        <tr>
          <th>სახელი</th>
          <th>შექმნის დრო</th>
          <th>ვის მიერ</th>
          <th>ტიპი</th>
          <th>სტატუსი</th>
          <th>მოქმედებები</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in items" :key="item.id">
          <td>{{ item.name }}</td>
          <td>{{ item.createdAt }}</td>
          <td>{{ item.createdBy }}</td>
          <td>{{ item.type }}</td>
          <td>{{ item.status }}</td>
          <td class="actions">
            <button class="btn" @click="$emit('view-details', item)">დათვალიერება</button>
            <button class="btn" @click="$emit('send-item', item)">გადაგზავნა</button>
            <button class="btn" @click="$emit('edit-item', item)">რედაქტირება</button>
            <button class="btn"  @click="$emit('delete-item', item)">წაშლა</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
const props = defineProps({ items: { type: Array, default: () => [] } })
</script>