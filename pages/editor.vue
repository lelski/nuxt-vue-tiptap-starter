<template>
  <client-only>
    <TiptapEditor v-model:htmlContent="htmlContent" />
  </client-only>
</template>

<script setup lang="ts">
const htmlContent = ref("");

watchEffect(() => {
  console.log(htmlContent.value);
});
</script>
