<template>
  <div class="node-children" :class="[`is__${layout}`, { 'is__top': isTop }]" :style="{ transform: isTop && `scale(${zoom})` }">
    <div class="node__wrapper">
      <div class="node__header">
        <div class="node-name">{{ orgData.name }}</div>
        <div class="node-type" v-if="orgData.type">{{ orgData.type }}</div>
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
        <div class="child-node" v-for="(child, index) in orgData.children" :key="index">            
          <div :class="[isVertical ? 'line-v__top' : 'line-h__left']" :ref="el => setRef(el)">
            <div class="line-h__joiner" v-if="index !== orgData.children.length - 1"></div>
          </div>
          <org-chart :org-data="child" :layout="layout" :is-top="false"></org-chart>
        </div>
      </div>
      <div class="children-line"></div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, unref, computed, watch, nextTick } from 'vue'
import html2canvas from 'html2canvas'

const emits = defineEmits(['zoom'])

const props = defineProps({
  orgData: Object,
  isTop: {
    type: Boolean,
    default: true
  },
  layout: String
})

// 当前的缩放等级
const zoom = ref(1)

const isVertical = computed(() => props.layout === 'vertical')

const hLineRefs = ref([])

// 根据line-v__top元素去画横线
function setRef(el) {
  if (!hLineRefs.value.includes(el)) {
    hLineRefs.value.push(el)
  }
}


function drawJoinLines() {
  const lines = unref(hLineRefs)
  for(let i = 0; i < lines.length - 1; i++) {
    const cur = lines[i]
    const next = lines[i + 1]
    const rect1 = cur.getBoundingClientRect()
    const rect2 = next.getBoundingClientRect()
    const { left, top, height } = rect1
    const { right, bottom, width } = rect2
    console.log(width, height)
    const el = cur.childNodes[0]
    if (!el || !el.style) {
      continue
    }
    if (unref(isVertical)) {
      el.style.width = Math.ceil((right - left) / unref(zoom)) + 'px'
      el.style.height = Math.ceil(width / unref(zoom)) + 'px'

      // el.style.width = (right - left) + 'px'
      // el.style.height = width + 'px'
    } else {
      el.style.height = Math.ceil((bottom - top) / unref(zoom)) + 'px'
      el.style.width = Math.ceil(height / unref(zoom)) + 'px'

      // el.style.height = (bottom - top) + 'px'
      // el.style.width = height + 'px'
    }
  }
}

// 放大
function zoomUp() {
  zoom.value = unref(zoom) + 0.1

  emits('zoom', unref(zoom))
}

// 缩小
function zoomDown() {
  if (unref(zoom) <= 0) return

  zoom.value = unref(zoom) - 0.1

  emits('zoom', unref(zoom))
}

// 展示完整的组织架构图
function showFull() {
  // 根据自身的实际尺寸和父元素的尺寸进行缩放
  const wrapper = document.querySelector('.node-children.is__top')
  const { width, height } = wrapper.getBoundingClientRect()
  const { width: _width, height: _height } = wrapper.parentNode.getBoundingClientRect()
  
  const zoomX = _width / width
  const zoomY = _height / height
  zoom.value = +((Math.min(zoomX, zoomY) - 0.01) * unref(zoom)).toFixed(3)

  emits('zoom', unref(zoom))
}

// 下载
function download() {
  const target = document.querySelector('.node-children.is__top')
  const { width, height } = target.getBoundingClientRect()
  html2canvas(target, {
    width, height,
    scale: Math.ceil(2 / unref(zoom)),
    backgroundColor: '#c6c6c6'
  }).then(function(canvas) {
    exportImage(canvas.toDataURL())
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

onMounted(drawJoinLines)

watch(() => props.layout, () => {
  nextTick(drawJoinLines)
})

defineExpose({
  zoomUp,
  zoomDown,
  showFull,
  download
})
</script>

<style lang="scss" scoped>
$lineColor: #ddd;
$gap: 20px;

.node-children {
  position: relative;
  width: fit-content;
  box-sizing: border-box;
  display: flex;
  align-items: center;

  &.is__top {
    padding: 10px;
    transform-origin: center center;
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

      .node-type {
        padding: 2px 8px;
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
    gap: $gap;
    align-items: flex-start;

    &.is__vertical {
      flex-direction: row;
    }

    &.is__horizontal {
      flex-direction: column;
    }

    .child-node {
      position: relative;

      .line-v__top {
        position: absolute;
        width: 2px;
        height: $gap;
        top: 0;
        transform: translateY(-100%);
        left: 50%;
        background-color: $lineColor;

        .line-h__joiner {
          background-color: inherit;
        }
      }

      .line-h__left {
        position: absolute;
        width: $gap;
        height: 2px;
        left: 0;
        transform: translateX(-100%);
        top: 50%;
        background-color: $lineColor;

        .line-h__joiner {
          background-color: inherit;
        }
      }
    }
  }
}
</style>