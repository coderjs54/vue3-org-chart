<template>
  <div class="node-wrapper">
    <div class="node">
      <div class="node-content">{{ orgData.name }}</div>
      <template v-if="orgData.children && orgData.children.length">
        <div class="node-children">
          <OrgChartNew v-for="(child, index) in orgData.children" :key="index" :orgData="child"></OrgChartNew>
        </div>
      </template>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  orgData: Object
})
</script>

<style lang="scss" scoped>
.node-wrapper {
  // display: flex;
  // flex-direction: row;
  // align-items: center;

  .node {
    position: relative;

    .node-content {
      width: 200px;
      height: 60px;
      background-color: #ddd;
      border-radius: 5px;
      margin-right: 40px;
    }

    .node-children {
      position: absolute;
      right: 0;
      top: 50%;
      transform: translateY(-50%);
    }
  }
}
</style>