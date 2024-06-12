<script setup>
const props = defineProps({
  msg: String
})
const emit = defineEmits(['response'])

emit('response', 'hello from child')
</script>

<template>
  <!-- <h2>{{ msg || 'ko the hien thi' }}</h2> -->
  <h1>Child component </h1>
  <slot>chua co gi de hien</slot>
</template>