<template>
  <div
    class="mr-1 inline-flex cursor-pointer content-center justify-center rounded-md px-2 py-1"
    :class="{ 'bg-slate-300': active, 'hover:bg-slate-200': !active }"
  >
    <slot />
  </div>
</template>

<script lang="ts" setup>
const { active = false } = defineProps<{ active?: boolean }>();
</script>

<style></style>
