<template>
  <div class="org-chart__wrapper">
    <div class="chart-wrapper">
      <OrgChart :orgData="orgData" ref="orgChartRef" :layout="layout" />
    </div>
    <div class="file-wrapper">
      <div class="file upload" @click="handleUploadFile">上传文件</div>
      <div class="file download" @click="handleDownloadTemplate">下载文件模板</div>
      <input hidden ref="fileInputRef" type="file" accept=".json" @input="handleFileInput">
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

const emits = defineEmits(['updateOrgData'])

const zoom = computed(() => orgChartRef && orgChartRef.value ? orgChartRef.value.zoom : 1)

const fileInputRef = ref(null)
const orgChartRef = ref(null)
const layouts = ['horizontal', 'vertical']
const layout = ref('vertical')

const handleUploadFile = () => {
  fileInputRef.value.click()
}

const handleFileInput = e => {
  const files = e.target.files
  console.log(files)
  if (!files.length) {
    return
  }

  const file = files[0]
  readFile(file).then(result => {
    try {
      const json = JSON.parse(result)
      emits('updateOrgData', json)
      handleShowFull()
    } catch(error) {
      alert('文件内容不合法')
    }
  }).catch(() => {})
}

const readFile = file => {
  return new Promise((resolve, reject) => {
    const reader = new FileReader()
    reader.onload = e => {
      resolve(e.target.result)
    }
    reader.onerror = reject
    reader.readAsText(file, 'utf-8')
  })
}

const handleDownloadTemplate = () => {
  const link = document.createElement('a')
  link.href = '/template.json'
  link.download = 'template.json'
  document.body.appendChild(link)
  link.click()
  link.remove()
}

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

  .file-wrapper {
    box-sizing: border-box;
    position: absolute;
    top: 5px;
    left: 10px;
    padding: 6px 8px;
    background-color: #fff;
    border-radius: 5px;
    user-select: none;
    display: flex;
    border: 1px solid #9fa4b2;
    box-shadow: 1px 2px 2px 0px #9fa4b2;

    .file {
      box-sizing: border-box;
      height: 20px;
      display: flex;
      justify-content: center;
      align-items: center;
      color: #333;
      font-size: 13px;
      font-weight: 500;
      cursor: pointer;      
      transition: all .2s linear;

      &:hover {
        color: #07845A;
      }

      &.upload {
        padding-right: 6px;
        border-right: 1px solid #BDC1CC;
      }

      &.download {
        padding-left: 6px;
      }
    }
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
      font-size: 13px;
      font-weight: 500;
      cursor: pointer;
      
      &:hover {
        color: #07845A;
      }
    }

    .add, .minus, .full, .download {
      border-bottom: 1px solid #BDC1CC;
      transition: all .2s linear;
    }
  }
}
</style>