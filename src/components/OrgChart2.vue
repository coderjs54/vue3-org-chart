<template>
  <div class="org-chart" :class="[`is__${layout}`, { 'is__top': isTop }]">
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
      <div class="children-wrapper" :ref="el => setCWrapperRef(el)" :class="[`is__${layout}`]">
        <div class="child-node" v-for="(child, index) in orgData.children" :key="index">            
          <div :class="[isVertical ? 'line-v__top' : 'line-h__left']" :ref="el => setRef(el)">
            <div class="line-h__joiner" v-if="index !== orgData.children.length - 1"></div>
          </div>  
          <org-chart :org-data="child" :layout="layout" :is-top="false"></org-chart>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, unref, computed, onUpdated } from 'vue'

const props = defineProps({
  orgData: Object,
  isTop: {
    type: Boolean,
    default: true
  },
  layout: {
    type: String,
    default: 'vertical',
    validator(val) {
      return ['vertical', 'horizontal'].includes(val)
    }
  }
})

const isVertical = computed(() => props.layout === 'vertical')

const hLineRefs = ref([])
const cWrapperRefs = ref([])

// 根据line-v__top元素去画横线
function setRef(el) {
  hLineRefs.value.push(el)
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
    const el = cur.childNodes[0]
    if (!el || !el.style) {
      continue
    }
    if (unref(isVertical)) {
      el.style.width = (right - left) + 'px'
      el.style.height = width + 'px'
    } else {
      el.style.height = (bottom - top) + 'px'
      el.style.width = height + 'px'
    }
  }
}

function setCWrapperRef(el) {
  cWrapperRefs.value.push(el)
}

onMounted(drawJoinLines)

onUpdated(drawJoinLines)
</script>

<style lang="scss" scoped>
$lineColor: yellowgreen;
$gap: 15px;

.org-chart {
  box-sizing: border-box;
  display: flex;
  align-items: center;

  &.is__horizontal {
    flex-direction: row;

    &.is__top {
      padding: 60px;
      overflow: auto;
    }

    .node__wrapper {
      margin-right: 60px;
    }
  }

  &.is__vertical {
    flex-direction: column;

    &.is__top {
      padding-top: 60px;
      overflow: auto;
    }

    .node__wrapper {
      margin-bottom: 60px;
    }
  }

  .node__wrapper {
    position: relative;
    width: 200px;
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
      background: #00c853;
      color: #fff;

      .node-name {
        color: #fff;
      }

      .node-type {
        padding: 2px 8px;
        background-color: #00838f;
        color: #fff;
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
      height: 30px;
      bottom: 0;
      transform: translateY(100%);
      left: 50%;
      background-color: $lineColor;
    }

    .line-h__right {
      position: absolute;
      width: 30px;
      height: 2px;
      right: 0;
      transform: translateX(100%);
      top: 50%;
      background-color: $lineColor;
    }
  }

  .children-wrapper {
    display: flex;
    gap: 30px;
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
        height: 30px;
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
        width: 30px;
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