<template>
  <div class="org-chart">
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
      <div class="line-v__bottom" v-if="orgData.children && orgData.children.length"></div>
    </div>
    <div v-if="orgData.children && orgData.children.length" class="node-children">
      <div class="children-wrapper">
        <div class="child-node" v-for="(child, index) in orgData.children" :key="index">            
          <div class="line__border" :class="{ border__last: index === orgData.children.length - 1 }"></div>
          <div
            class="line-h__top"
            :class="{ 
              is_first: index === 0,
              is__last: index === orgData.children.length - 1,
            }"
            v-if="child.children && child.children.length">
          </div>
          <org-chart :org-data="child"></org-chart>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  orgData: Object
})
</script>

<style lang="scss" scoped>
$lineColor: #ddd;
$gap: 15px;

.org-chart {
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  align-items: center;

  .node__wrapper {
    position: relative;
    width: 200px;
    background: #fff;
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
    border-radius: 5px;
    margin-bottom: 60px;
    cursor: pointer;

    .node__header {
      padding: 5px 12px;
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
        padding: 2px 4px;
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
  }

  .children-wrapper {
    display: flex;
    gap: 30px;

    .child-node {
      position: relative;

      .line__border {
        position: absolute;
        width: calc(100% + $gap + 15px);
        height: 30px;
        top: 0;
        transform: translateY(-100%);
        left: 50%;
        border-left: 2px solid $lineColor;
        border-top: 2px solid $lineColor;

        &.border__last {
          width: 50%;
          left: 0;
          border-left: none;
          border-right: 2px solid $lineColor;
        }
      }

      .line-h__top {
        position: absolute;
        width: 50%;
        height: 30px;
        top: 0;
        transform: translateY(-100%);
        right: 50%;
        // border-right: 2px solid $lineColor;
        border-top: 2px solid $lineColor;
        
        &.is_first {
          border: none;
        }
        &.is__last {
          border-right: none;
        }
      }
    }
  }
}
</style>