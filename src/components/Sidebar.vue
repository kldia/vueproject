<template>
  <aside :class="['sidebar', { collapsed }]">
    <nav>
      <ul>
        <li>
          <a class="side-link" href="#">
            <img src="../assets/home.svg" alt="logo" />
            <span>Dashboard</span>
          </a>
        </li>
        <li>
          <a class="side-link" href="#">
            <img src="../assets/documents.svg" alt="logo" />
            <span>Orders</span>
          </a>
        </li>
        <li>
          <a class="side-link" href="#">
            <img src="../assets/calculator.svg" alt="logo" />
            <span>Products</span>
          </a>
        </li>
        <li>
          <a class="side-link" href="#">
            <img src="../assets/settings.svg" alt="logo" />
            <span>Settings</span>
          </a>
        </li>
      </ul>
    </nav>
    <button class="toggle" @click="$emit('toggle')">
      {{ collapsed ? '>' : '<' }}
    </button>
  </aside>
</template>

<script setup>
import { toRef } from 'vue'
const props = defineProps({ collapsed: { type: Boolean, default: false } })
</script>

<style scoped>
ul { list-style-type: none;}
a { text-decoration: none; color: #fff}
.side-link{
  display: flex;
  align-items: center;
  gap: 8px;}



</style>