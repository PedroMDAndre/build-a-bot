<template>
  <div>
    <div class="header">
      <span v-if="open" @click="toggleOpen" @keydown.enter="toggleOpen">
        <slot name="collapse">&#x25B2; Collapse</slot>
      </span>

      <span v-else @click="toggleOpen" @keydown.enter="toggleOpen">
        <slot name="expand">&#x25BC; Expand</slot>
      </span>
    </div>
    <slot v-if="open"></slot>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const open = ref(true);
const toggleOpen = () => {
  open.value = !open.value;
};
</script>

<style lang="scss" scoped>
.header {
  background-color: #bbb;
  padding: 3px;
  cursor: pointer;
  font-size: 22px;
}
</style>
