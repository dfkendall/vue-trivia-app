<script setup>
const props = defineProps({ text: String, fontSize: String })
</script>

<template>
  <h2 class="subtitle-text" :style="`font-size: ${fontSize}px`">{{ text }}</h2>
</template>
