<script setup lang="ts">
import { $t } from '@/locales';
import { useEcharts } from '@/hooks/common/echarts';
import {
  barOptions,
} from './data';
defineOptions({
  name: 'CreativityBanner'
});

const { domRef: barRef } = useEcharts(() => barOptions, { onRender() { } });
</script>

<template>
  <NCard :bordered="false" size="small" class="h-full card-wrapper">
    <NCard :bordered="false" class="card-wrapper">
      <div style="font-size:17px;text-align: center; font-weight: bold;">流量环比上升</div>
      <div ref="barRef" class="h-400px" />
    </NCard>
  </NCard>
</template>

<style scoped></style>
