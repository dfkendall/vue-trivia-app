<script setup>
const props = defineProps({ text: String, fontSize: String })
</script>

<template>
  <h1 class="title-text reveal" :style="`font-size: ${fontSize}px`">{{ text }}</h1>
</template>
