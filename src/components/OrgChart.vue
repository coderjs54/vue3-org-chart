<template>
  <div
    class="node-children"
    :ref="el => setWrapperRef(el)"
    :class="[`is__${layout}`, { isTop }]"
    :style="{ zoom: isTop && zoom, transform: isTop && transformStyle }"
  >
    <div class="node__wrapper">
      <div class="node__header">
        <div class="node-name" :title="orgData.name">{{ orgData.name }}</div>
        <div class="node-type" :title="orgData.type" v-if="orgData.type">{{ orgData.type }}</div>
      </div>
      <div class="node__info">
        <div class="info-item">部门编码：{{ orgData.code }}</div>
        <div class="info-item">负责人：{{ orgData.manager }}</div>
        <div class="info-item">成员数：{{ orgData.members || 0 }}</div>
      </div>
      <template v-if="orgData.children && orgData.children.length">
        <div v-if="isVertical" class="line-v__bottom"></div>
        <div v-else class="line-h__right"></div>
      </template>
    </div>
    <div v-if="orgData.children && orgData.children.length" class="node-children">
      <div class="children-wrapper" :class="[`is__${layout}`]">
        <div
          class="child-node"
          :class="{ isVertical: isVertical, isHorizontal: !isVertical}"
          v-for="(child, index) in orgData.children"
          :key="index"
        >            
          <div :class="[isVertical ? 'line-v__top' : 'line-h__left']"></div>
          <org-chart :org-data="child" :layout="layout" :is-top="false"></org-chart>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, unref, computed, onMounted, nextTick, watch } from 'vue'
import html2canvas from 'html2canvas'

const props = defineProps({
  orgData: Object,
  isTop: {
    type: Boolean,
    default: true
  },
  layout: String,
})

const wrapperRef = ref(null)

// 当前的缩放等级
const zoom = ref(1)

const transform = ref({
  translateX: 0,
  translateY: 0
})

const transformStyle = computed(() => {
  const { translateX, translateY } = unref(transform)
  return `translate(${translateX}, ${translateY})`
})

const isVertical = computed(() => props.layout === 'vertical')

function setWrapperRef(el) {
  if (props.isTop) {
    wrapperRef.value = el
  } 
}

// 放大
function zoomUp() {
  zoom.value = +(unref(zoom) + 0.1).toFixed(1)

  nextTick(centerlize)
}

// 缩小
function zoomDown() {
  if (unref(zoom) <= 0.1) return

  // 始终保留一位小数 避免小数出现精度问题
  zoom.value = +(unref(zoom) - 0.1).toFixed(1)

  nextTick(centerlize)
}

// 展示完整的组织架构图
function showFull() {
  // 根据自身的实际尺寸和父元素的尺寸进行缩放
  const wrapper = wrapperRef.value
  const { width, height } = wrapper.getBoundingClientRect()
  const { width: _width, height: _height } = wrapper.parentNode.getBoundingClientRect()
  
  const zoomX = _width / width
  const zoomY = _height / height
  zoom.value = +((Math.min(zoomX, zoomY) * unref(zoom)).toFixed(4))

  nextTick(centerlize)
}

// 下载
function download() {
  const target = wrapperRef.value
  let cloned = target.cloneNode(true)
  cloned.style.zoom = 1
  cloned.style.position = 'fixed'
  cloned.style.top = '-9999px'
  cloned.style.left = '-9999px'
  document.body.appendChild(cloned)
  const { width, height } = cloned.getBoundingClientRect()
  html2canvas(cloned, {
    width, height,
    scale: 2,
    backgroundColor: '#f6f6f6'
  }).then(function(canvas) {
    exportImage(canvas.toDataURL())
    cloned.remove()
    cloned = null
  })
}

// 工具函数：导出图片
function exportImage(base64) {
  const link = document.createElement('a')
  link.href = base64
  link.download = 'chart.png'
  document.body.appendChild(link)
  link.click()
  link.remove()
}

// 计算自身的尺寸与父元素的尺寸，居中显示内容
function centerlize() {
  if (!props.isTop) return

  const nodeWrapper = wrapperRef.value
  const parentNode = nodeWrapper.parentNode
  const { width, height } = nodeWrapper.getBoundingClientRect()
  const { width: width1, height: height1 } = parentNode.getBoundingClientRect()
  if (width < width1) {
    transform.value.translateX = `${(width1 - width) / 2 / unref(zoom)}px`
  }
  if (height < height1) {
    transform.value.translateY = `${(height1 - height) / 2 / unref(zoom)}px`
  }
}

onMounted(centerlize)

// 布局方向变化后需要重新进行居中显示的计算
watch(() => props.layout, () => {
  transform.value.translateX = 0
  transform.value.translateY = 0
  nextTick(centerlize)
})

defineExpose({
  zoom,
  zoomUp,
  zoomDown,
  showFull,
  download
})
</script>

<style lang="scss" scoped>
$lineColor: #ddd;
$gap: 20px;
$width: 200px;

.node-children {
  position: relative;
  width: fit-content;
  box-sizing: border-box;
  display: flex;
  align-items: center;

  &.isTop {
    padding: 10px;
  }

  &.is__horizontal {
    flex-direction: row;

    .node__wrapper {
      margin-right: $gap * 2;
    }
  }

  &.is__vertical {
    flex-direction: column;

    .node__wrapper {
      margin-bottom: $gap * 2;
    }
  }

  .children-line {
    position: absolute;
    background-color: $lineColor;
  }

  .node__wrapper {
    position: relative;
    min-width: 180px;
    width: 180px;
    background: #fff;
    box-shadow: 2px 2px 3px rgba(0, 0, 0, 0.1);
    border-radius: 5px;
    cursor: pointer;

    .node__header {
      padding: 5px 8px 5px 12px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      border-radius: 5px 5px 0 0;
      background: #07845A;
      color: #fff;

      .node-name {
        white-space: nowrap;
        text-overflow: ellipsis;
        overflow: hidden;
        padding-right: 4px;
      }

      .node-type {
        padding: 2px 5px;
        white-space: nowrap;
        background-color: #57B191;
        border-radius: 5px;
        font-size: 14px;
      }
    }

    .node__info {
      padding: 5px 12px;
      font-size: 14px;
      .info-item {
        text-align: left;
        margin-bottom: 5px;
        white-space: nowrap;
        text-overflow: ellipsis;
        overflow: hidden;

        &:last-of-type {
          margin-bottom: 0;
        }
      }
    }

    .line-v__bottom {
      position: absolute;
      width: 2px;
      height: $gap;
      bottom: 0;
      transform: translateY(100%);
      left: 50%;
      background-color: $lineColor;
    }

    .line-h__right {
      position: absolute;
      width: $gap;
      height: 2px;
      right: 0;
      transform: translateX(100%);
      top: 50%;
      background-color: $lineColor;
    }
  }

  .children-wrapper {
    display: flex;
    align-items: flex-start;

    &.is__vertical {
      flex-direction: row;
    }

    &.is__horizontal {
      flex-direction: column;
    }

    .child-node {
      position: relative;

      &.isVertical {
        padding-left: $gap;
        padding-right: $gap;

        &:first-of-type {
          padding-left: 0;
        }

        &:last-of-type {
          padding-right: 0;
        }

        &::before {
          content: '';
          position: absolute;
          left: 0;
          top: -$gap;
          width: 50%;
          height: 2px;
          background-color: $lineColor;
        }

        &::after {
          content: '';
          position: absolute;
          right: 0;
          top: -$gap;
          width: 50%;
          height: 2px;
          background-color: $lineColor;
        }
      }

      &.isHorizontal {
        padding-top: $gap;
        padding-bottom: $gap;

        &:first-of-type {
          padding-top: 0;
        }

        &:last-of-type {
          padding-bottom: 0;
        }

        &::before {
          content: '';
          position: absolute;
          top: 0;
          left: -$gap;
          height: 50%;
          width: 2px;
          background-color: $lineColor;
        }

        &::after {
          content: '';
          position: absolute;
          bottom: 0;
          left: -$gap;
          height: 50%;
          width: 2px;
          background-color: $lineColor;
        }
      }

      &:nth-of-type(1) {
        &::before {
          background-color: transparent;
        }
      }

      &:last-of-type {
        &::after {
          background-color: transparent;
        }
      }

      .line-v__top {
        position: absolute;
        width: 2px;
        height: $gap;
        top: 0;
        transform: translateY(-100%);
        left: 50%;
        background-color: $lineColor;
      }

      .line-h__left {
        position: absolute;
        width: $gap;
        height: 2px;
        left: 0;
        transform: translateX(-100%);
        top: 50%;
        background-color: $lineColor;
      }
    }
  }
}
</style>