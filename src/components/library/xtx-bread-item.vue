<template>
  <div class="xtx-bread-item">
    <RouterLink v-if="to" :to="to"><slot /></RouterLink>
    <span v-else><slot /></span>
  </div>
</template>
<script setup>
import { ref } from 'vue'
const props = defineProps({
  to: {
    type: [String, Object],
    default: '',
  },
})
</script>

<style scoped lang="less"></style>
