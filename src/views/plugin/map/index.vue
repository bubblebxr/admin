<script setup lang="ts">
import { TencentMap } from './components';
</script>

<template>
  <div class="h-full">
    <NCard title="设备信息的可视化展示" :bordered="false" class="h-full card-wrapper" content-style="overflow:hidden">
      <TencentMap />
    </NCard>
  </div>
</template>

<style scoped></style>
