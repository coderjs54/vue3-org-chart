<template>
  <div class="org-chart__wrapper">
    <div class="chart-wrapper">
      <OrgChart :orgData="orgData" ref="orgChartRef" :layout="layout" />
    </div>
    <div class="zoom-wrapper">当前缩放：{{ zoom.toFixed(2) }}</div>
    <div class="tool-wrapper">
      <div title="放大" class="tool add" @click.stop="handleClickAdd">放大</div>
      <div title="缩小" class="tool minus" @click.stop="handleClickMinus">缩小</div>
      <div title="自适应" class="tool full" @click.stop="handleShowFull">适应</div>
      <div title="下载" class="tool download" @click.stop="handleDownload">保存</div>
      <div title="视图" class="tool change" @click.stop="handleChange">视图</div>
    </div>
  </div>
</template>

<script setup>
import { ref, unref, computed } from 'vue'
import OrgChart from './OrgChart.vue'

const props = defineProps({
  orgData: Object
})

const zoom = computed(() => orgChartRef && orgChartRef.value ? orgChartRef.value.zoom : 1)

const orgChartRef = ref(null)
const layouts = ['horizontal', 'vertical']
const layout = ref('vertical')

const handleClickAdd = () => {
  orgChartRef.value.zoomUp()
}

const handleClickMinus = () => {
  orgChartRef.value.zoomDown()
}

const handleShowFull = () => {
  orgChartRef.value.showFull()
}

const handleDownload = () => {
  orgChartRef.value.download()
}

const handleChange = () => {
  let i = layouts.indexOf(unref(layout))
  layout.value = layouts[++i % 2]
}
</script>

<style lang="scss" scoped>
.org-chart__wrapper {
  position: relative;
  margin: 0px auto;
  width: 100%;
  height: 100%;

  .chart-wrapper {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    overflow: auto;
    background-color: #F5F5F5;
  }

  .zoom-wrapper {
    box-sizing: border-box;
    position: absolute;
    top: 5px;
    right: 25px;
    padding: 4px 6px;
    background-color: #fff;
    border-radius: 5px;
    display: flex;
    justify-content: center;
    align-items: center;
    color: #333;
    user-select: none;
    font-size: 12px;
    box-shadow: 1px 2px 2px 0px #9fa4b2;
  }

  .tool-wrapper {
    box-sizing: border-box;
    position: absolute;
    bottom: 25px;
    right: 25px;
    padding: 0 4px;
    background-color: #fff;
    border-radius: 5px;
    display: flex;
    flex-direction: column;
    align-items: center;
    user-select: none;
    border: 1px solid #9fa4b2;
    box-shadow: 1px 2px 2px 0px #9fa4b2;

    .tool {
      width: 35px;
      height: 35px;
      display: flex;
      justify-content: center;
      align-items: center;
      color: #333;
      font-size: 12px;
      font-weight: 500;
      cursor: pointer;
    }

    .add, .minus, .full, .download {
      border-bottom: 1px solid #BDC1CC;
    }
  }
}
</style>