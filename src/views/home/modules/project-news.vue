<script setup lang="ts">
import { watch } from 'vue';
import { computed } from 'vue';
import { $t } from '@/locales';
import { useAppStore } from '@/store/modules/app';
import { useEcharts } from '@/hooks/common/echarts';
defineOptions({
  name: 'LineChart'
});

const appStore = useAppStore();

interface NewsItem {
  id: number;
  content: string;
  time: string;
}

// const newses = computed<NewsItem[]>(() => [
//   { id: 1, content: $t('page.home.projectNews.desc1'), time: '2021-05-28 22:22:22' },
//   { id: 2, content: $t('page.home.projectNews.desc2'), time: '2021-10-27 10:24:54' },
//   { id: 3, content: $t('page.home.projectNews.desc3'), time: '2021-10-31 22:43:12' },
//   { id: 4, content: $t('page.home.projectNews.desc4'), time: '2021-11-03 20:33:31' },
//   { id: 5, content: $t('page.home.projectNews.desc5'), time: '2021-11-07 22:45:32' }
// ]);

const { domRef, updateOptions } = useEcharts(() => ({
  tooltip: {
    trigger: 'axis',
    axisPointer: {
      type: 'cross',
      label: {
        backgroundColor: '#6a7985'
      }
    }
  },
  legend: {
    data: [$t('page.home.lastWeekFlowRate'), $t('page.home.lastLastWeekFlowRate')]
  },
  grid: {
    left: '3%',
    right: '4%',
    bottom: '3%',
    containLabel: true
  },
  xAxis: {
    type: 'category',
    boundaryGap: false,
    data: [] as string[]
  },
  yAxis: {
    type: 'value'
  },
  series: [
    {
      color: '#8e9dff',
      name: $t('page.home.lastWeekFlowRate'),
      type: 'line',
      smooth: true,
      stack: 'Total',
      areaStyle: {
        color: {
          type: 'linear',
          x: 0,
          y: 0,
          x2: 0,
          y2: 1,
          colorStops: [
            {
              offset: 0.25,
              color: '#8e9dff'
            },
            {
              offset: 1,
              color: '#fff'
            }
          ]
        }
      },
      emphasis: {
        focus: 'series'
      },
      data: [] as number[]
    },
    {
      color: '#26deca',
      name: $t('page.home.lastLastWeekFlowRate'),
      type: 'line',
      smooth: true,
      stack: 'Total',
      areaStyle: {
        color: {
          type: 'linear',
          x: 0,
          y: 0,
          x2: 0,
          y2: 1,
          colorStops: [
            {
              offset: 0.25,
              color: '#26deca'
            },
            {
              offset: 1,
              color: '#fff'
            }
          ]
        }
      },
      emphasis: {
        focus: 'series'
      },
      data: []
    }
  ]
}));
async function mockData() {
  await new Promise(resolve => {
    setTimeout(resolve, 1000);
  });

  updateOptions(opts => {
    opts.xAxis.data = ['06:00', '08:00', '10:00', '12:00', '14:00', '16:00', '18:00', '20:00', '22:00', '24:00'];
    opts.series[0].data = [4623, 6145, 6268, 6411, 1890, 4251, 2978, 3880, 3606, 4311];
    opts.series[1].data = [2208, 2016, 2916, 4512, 8281, 2008, 1963, 2367, 2956, 678];

    return opts;
  });
}

function updateLocale() {
  updateOptions((opts, factory) => {
    const originOpts = factory();

    opts.legend.data = originOpts.legend.data;
    opts.series[0].name = originOpts.series[0].name;
    opts.series[1].name = originOpts.series[1].name;

    return opts;
  });
}

async function init() {
  mockData();
}

watch(
  () => appStore.locale,
  () => {
    updateLocale();
  }
);

// init
init();
</script>

<template>
  <NCard :bordered="false" size="small" segmented class="card-wrapper">
    <!-- <template #header-extra>
      <a class="text-primary" href="javascript:;">{{ $t('page.home.projectNews.moreNews') }}</a>
    </template>
    <NList>
      <NListItem v-for="item in newses" :key="item.id">
        <template #prefix>
          <SoybeanAvatar class="size-48px!" />
        </template>
        <NThing :title="item.content" :description="item.time" />
      </NListItem>
    </NList> -->
    <div style="font-size:17px;text-align: center; font-weight: bold;">流量峰值环比分析</div>
    <NCard :bordered="false" class="card-wrapper">
      <div ref="domRef" class="h-360px overflow-hidden"></div>
    </NCard>
  </NCard>

</template>

<style scoped></style>
