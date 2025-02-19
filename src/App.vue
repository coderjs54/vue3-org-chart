<script setup>
import { ref } from 'vue'
import OrgChartWrapper from './components/OrgChartBox.vue'

const orgData = ref({
  name: '杭州银行科技部',
  members: 7,
  type: '部门',
  code: '0000',
  manager: '刘德华',
  children: [
    {
      name: '采购部',
      manager: '石雷',
      type: '部门',
      members: 1,
      code: '00003',
    },
    {
      name: '直属成员',
      manager: '暂无负责人',
      type: '直属成员',
      members: 3,
      code: '00001',
      children: [
        {
          name: '直属成员',
          manager: '暂无负责人',
          type: '直属成员',
          members: 1,
          code: '000041',
        },
        {
          name: '厨师组',
          manager: '暂无负责人',
          type: '部门',
          members: 0,
          code: '000042',
        },
        {
          name: '销售组',
          manager: '暂无负责人',
          type: '部门',
          members: 1,
          code: '000043',
        }
      ]
    },
    {
      name: '财务部',
      manager: '黄俊',
      type: '部门',
      members: 0,
      code: '00002',
      children: [
        {
          name: '直属成员',
          manager: '暂无负责人',
          type: '直属成员',
          members: 1,
          code: '000041',
      children: [
        {
          name: '直属成员',
          manager: '暂无负责人',
          type: '直属成员',
          members: 1,
          code: '000041',
        },
        {
          name: '厨师组',
          manager: '暂无负责人',
          type: '部门',
          members: 0,
          code: '000042',
        },
        {
          name: '销售组',
          manager: '暂无负责人',
          type: '部门',
          members: 1,
          code: '000043',
      children: [
        {
          name: '直属成员',
          manager: '暂无负责人',
          type: '直属成员',
          members: 1,
          code: '000041',
        },
        {
          name: '厨师组',
          manager: '暂无负责人',
          type: '部门',
          members: 0,
          code: '000042',
        },
        {
          name: '销售组',
          manager: '暂无负责人',
          type: '部门',
          members: 1,
          code: '000043',
      children: [
        {
          name: '直属成员',
          manager: '暂无负责人',
          type: '直属成员',
          members: 1,
          code: '000041',
        },
        {
          name: '厨师组',
          manager: '暂无负责人',
          type: '部门',
          members: 0,
          code: '000042',
        },
        {
          name: '销售组',
          manager: '暂无负责人',
          type: '部门',
          members: 1,
          code: '000043',
        }
      ]
        }
      ]
        }
      ]
        },
        {
          name: '厨师组',
          manager: '暂无负责人',
          type: '部门',
          members: 0,
          code: '000042',
        },
        {
          name: '销售组',
          manager: '暂无负责人',
          type: '部门',
          members: 1,
          code: '000043',
      children: [
        {
          name: '直属成员',
          manager: '暂无负责人',
          type: '直属成员',
          members: 1,
          code: '000041',
      children: [
        {
          name: '直属成员',
          manager: '暂无负责人',
          type: '直属成员',
          members: 1,
          code: '000041',
        },
        {
          name: '厨师组',
          manager: '暂无负责人',
          type: '部门',
          members: 0,
          code: '000042',
        },
        {
          name: '销售组',
          manager: '暂无负责人',
          type: '部门',
          members: 1,
          code: '000043',
      children: [
        {
          name: '直属成员',
          manager: '暂无负责人',
          type: '直属成员',
          members: 1,
          code: '000041',
        },
        {
          name: '厨师组',
          manager: '暂无负责人',
          type: '部门',
          members: 0,
          code: '000042',
        },
        {
          name: '销售组',
          manager: '暂无负责人',
          type: '部门',
          members: 1,
          code: '000043',
      children: [
        {
          name: '直属成员',
          manager: '暂无负责人',
          type: '直属成员',
          members: 1,
          code: '000041',
        },
        {
          name: '厨师组',
          manager: '暂无负责人',
          type: '部门',
          members: 0,
          code: '000042',
        },
        {
          name: '销售组',
          manager: '暂无负责人',
          type: '部门',
          members: 1,
          code: '000043',
        }
      ]
        }
      ]
        }
      ]
        },
        {
          name: '厨师组',
          manager: '暂无负责人',
          type: '部门',
          members: 0,
          code: '000042',
        },
        {
          name: '销售组',
          manager: '暂无负责人',
          type: '部门',
          members: 1,
          code: '000043',
        }
      ]
        }
      ]
    },
    {
      name: '生产管理部',
      manager: '管文俊',
      type: '部门',
      members: 3,
      code: '00004',
    }
  ]
})

const orgDataNew = ref({
  name: '江苏省',
  members: 7,
  type: '部门',
  code: '0000',
  manager: '刘德华',
  children: [
    {
      name: '淮安市',
      manager: '石雷',
      type: '部门',
      members: 1,
      code: '00003',
      children: [
        {
          name: '楚州',
          manager: '石雷',
          type: '部门',
          members: 100,
          code: '000031',
        },
        {
          name: '涟水',
          manager: '石雷',
          type: '部门',
          members: 1,
          code: '000032',
        },
        {
          name: '淮阴',
          manager: '石雷',
          type: '部门',
          members: 1,
          code: '000033',
          children: [
            {
              name: '高家堰',
              manager: '石雷',
              type: '部门',
              members: 1,
              code: '0000331',
            }
          ]
        },        
        // {
        //   name: '青浦',
        //   manager: '石雷',
        //   type: '部门',
        //   members: 1,
        //   code: '000034',
        // },
      ]
    },
    {
      name: '南京市',
      manager: '暂无负责人',
      type: '直属成员',
      members: 3,
      code: '00001',
      children: [
        {
          name: '栖霞区',
          manager: '暂无负责人',
          type: '直属成员',
          members: 1,
          code: '000041',
        },
        {
          name: '秦淮区',
          manager: '暂无负责人',
          type: '部门',
          members: 0,
          code: '000042',
        },
        {
          name: '雨花区',
          manager: '暂无负责人',
          type: '部门',
          members: 1,
          code: '000043',
          children: [
            {
              name: '白下',
              manager: '暂无负责人',
              type: '直属成员',
              members: 1,
              code: '0000421',
            }
          ]
        }
      ]
    },
    {
      name: '镇江市',
      manager: '黄俊',
      type: '部门',
      members: 0,
      code: '00002',
    },
    {
      name: '常州市',
      manager: '管文俊',
      type: '部门',
      members: 3,
      code: '00004',
    }
  ]
})

const orgDataMore = ref({
  "name": "中国",
  "type": "国家",
  "children": [
    {
      "name": "北京市",
      "type": "直辖市",
      "manager": "暂无负责人",
      "members": 20,
      "code": "0100",
      "children": [
        {
          "name": "东城区",
          "type": "区",
          "manager": "张三",
          "members": 5,
          "code": "0101"
        },
        {
          "name": "西城区",
          "type": "区",
          "manager": "李四",
          "members": 6,
          "code": "0102"
        }
      ]
    },
    {
      "name": "天津市",
      "type": "直辖市",
      "manager": "暂无负责人",
      "members": 18,
      "code": "0200",
      "children": [
        {
          "name": "和平区",
          "type": "区",
          "manager": "王五",
          "members": 4,
          "code": "0201"
        },
        {
          "name": "河东区",
          "type": "区",
          "manager": "赵六",
          "members": 5,
          "code": "0202"
        }
      ]
    },
    {
      "name": "河北省",
      "type": "省",
      "manager": "孙七",
      "members": 50,
      "code": "0300",
      "children": [
        {
          "name": "石家庄市",
          "type": "市",
          "manager": "周八",
          "members": 10,
          "code": "0301",
          "children": [
            {
              "name": "长安区",
              "type": "区",
              "manager": "吴九",
          "members": 3,
          "code": "030101"
            },
            {
              "name": "桥西区",
              "type": "区",
              "manager": "郑十",
          "members": 3,
          "code": "030102"
            }
          ]
        },
        {
          "name": "唐山市",
          "type": "市",
          "manager": "冯十一",
          "members": 8,
          "code": "0302",
          "children": [
            {
              "name": "路北区",
              "type": "区",
              "manager": "陈十二",
          "members": 2,
          "code": "030201"
            },
            {
              "name": "路南区",
              "type": "区",
              "manager": "褚十三",
          "members": 2,
          "code": "030202"
            }
          ]
        }
      ]
    },
    // 其他省份、自治区、直辖市、特别行政区数据依次类推，由于数据量较大，此处省略部分内容
    {
      "name": "香港特别行政区",
      "type": "特别行政区",
      "manager": "暂无负责人",
      "members": 12,
      "code": "3300",
      "children": [
        {
          "name": "香港岛",
          "type": "区",
          "manager": "暂无负责人",
          "members": 4,
          "code": "3301"
        },
        {
          "name": "九龙",
          "type": "区",
          "manager": "暂无负责人",
          "members": 4,
          "code": "3302"
        },
        {
          "name": "新界",
          "type": "区",
          "manager": "暂无负责人",
          "members": 4,
          "code": "3303"
        }
      ]
    },
    {
      "name": "澳门特别行政区",
      "type": "特别行政区",
      "manager": "暂无负责人",
      "members": 8,
      "code": "3400",
      "children": [
        {
          "name": "澳门半岛",
          "type": "区",
          "manager": "暂无负责人",
          "members": 4,
          "code": "3401"
        },
        {
          "name": "氹仔岛",
          "type": "区",
          "manager": "暂无负责人",
          "members": 2,
          "code": "3402"
        },
        {
          "name": "路环岛",
          "type": "区",
          "manager": "暂无负责人",
          "members": 2,
          "code": "3403"
        }
      ]
    }
  ]
})
</script>

<template>
  <div class="wrapper">
    <OrgChartWrapper :orgData="orgDataMore" />
  </div>
</template>

<style scoped>
.wrapper {
  box-sizing: border-box;
  width: 100%;
  height: 100vh;
}
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
