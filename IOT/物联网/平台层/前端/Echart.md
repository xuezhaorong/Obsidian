> 官网链接：https://echarts.apache.org/zh/index.html
> 官网链接：https://github.com/ecomfe/vue-echarts/blob/HEAD/README.zh-Hans.md

## 安装
```shell
npm install echarts vue-echarts
```

## 导入
```js
// 引入 echarts
import ECharts from 'vue-echarts'
// 导入 ECharts 核心组件和渲染器
import * as echarts from 'echarts/core';
import {
  CanvasRenderer
} from 'echarts/renderers';
// 导入需要的图例
import {
  LineChart
} from 'echarts/charts';

import {
  TitleComponent,
  TooltipComponent,
  GridComponent,
  DatasetComponent,
  LegendComponent
} from 'echarts/components';

// 注册必要的组件
echarts.use([
  CanvasRenderer,
  LineChart,
  TitleComponent,
  TooltipComponent,
  GridComponent,
  DatasetComponent,
  LegendComponent
]);

app.component('v-chart', ECharts)

// uni app
export function createApp() {
	... app.component('v-chart', ECharts)
}
```

## 使用
```vue
<template>
  <v-chart class="chart" :option="option" />
</template>

<script setup>
import { use } from "echarts/core";
import { CanvasRenderer } from "echarts/renderers";
import { PieChart } from "echarts/charts";
import {
  TitleComponent,
  TooltipComponent,
  LegendComponent
} from "echarts/components";
import VChart, { THEME_KEY } from "vue-echarts";
import { ref, provide } from "vue";

use([
  CanvasRenderer,
  PieChart,
  TitleComponent,
  TooltipComponent,
  LegendComponent
]);

provide(THEME_KEY, "dark");

const option = ref({
  title: {
    text: "Traffic Sources",
    left: "center"
  },
  tooltip: {
    trigger: "item",
    formatter: "{a} <br/>{b} : {c} ({d}%)"
  },
  legend: {
    orient: "vertical",
    left: "left",
    data: ["Direct", "Email", "Ad Networks", "Video Ads", "Search Engines"]
  },
  series: [
    {
      name: "Traffic Sources",
      type: "pie",
      radius: "55%",
      center: ["50%", "60%"],
      data: [
        { value: 335, name: "Direct" },
        { value: 310, name: "Email" },
        { value: 234, name: "Ad Networks" },
        { value: 135, name: "Video Ads" },
        { value: 1548, name: "Search Engines" }
      ],
      emphasis: {
        itemStyle: {
          shadowBlur: 10,
          shadowOffsetX: 0,
          shadowColor: "rgba(0, 0, 0, 0.5)"
        }
      }
    }
  ]
});
</script>

<style scoped>
.chart {
  height: 400px;
}
</style>
```