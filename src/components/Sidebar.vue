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

<style lang="scss" scoped>

@import  "../styles/_base.scss";

.sidebar {
  width: 200px;
  transition: width 0.3s ease;
  background: $base;
  overflow: hidden;
  border-right: 1px solid $border;
  padding: 12px;
  color: #fff;

  &.collapsed {
    width: 60px;

    .side-link {
      span {
        opacity: 0;
        transform: translateX(-10px);
      }
    }
  }

  ul {
    list-style: none;
    padding: 0;
    margin: 0;
  }

  .side-link {
    display: flex;
    align-items: center;
    gap: 8px;
    padding: 10px;
    text-decoration: none;
    color: inherit;

    img {
      width: 24px;
      height: 24px;
    }

    span {
      white-space: nowrap;
      opacity: 1;
      transform: translateX(0);
      transition: opacity 0.3s ease, transform 0.3s ease;
    }
  }

  .toggle {
    width: 100%;
    background: none;
    border: none;
    color: inherit;
    cursor: pointer;
    padding: 10px;
    text-align: center;
  }
}

ul {
  list-style-type: none;
}

a {
  text-decoration: none;
  color: #fff;
}

.side-link {
  display: flex;
  align-items: center;
  gap: 8px;
}




</style>