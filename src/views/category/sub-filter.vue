<template>
  <div class="sub-filter" v-if="!filterLoading">
    <div class="item">
      <div class="head">品牌：</div>
      <div class="body">
        <a
          @click="changeBrand(item.id)"
          :class="{ active: item.id === filterData.brands.selectedBrand }"
          href="javascript:;"
          v-for="item in filterData.brands"
          :key="item.id"
          >{{ item.name }}</a
        >
      </div>
    </div>
    <div class="item" v-for="item in filterData.saleProperties" :key="item.id">
      <div class="head">{{ item.name }}：</div>
      <div class="body">
        <a
          @click="changeProp(item, prop.id)"
          href="javascript:;"
          v-for="prop in item.properties"
          :key="prop.id"
          :class="{ active: prop.id === item.selectedAttr }"
          >{{ prop.name }}</a
        >
      </div>
    </div>
  </div>
  <div v-else class="sub-filter skl">
    <XtxSkeleton
      v-for="i in 5"
      animated
      class="item skl-item"
      width="800px"
      height="20px"
    />
  </div>
</template>
<script setup>
import { findSubCategoryFilter } from '@/api/category'
import XtxSkeleton from '@/components/library/xtx-skeleton.vue'
import { ref, watch } from 'vue'
import { useRoute } from 'vue-router'
const emit = defineEmits(['filter-change'])
const route = useRoute()
// 监听二级类目ID的变化获取筛选数据
const filterData = ref(null)
const filterLoading = ref(false)

// 监听 路由id 变化
watch(
  () => route.params.id,
  async (newVal) => {
    // 变化后的Id有值 且 处在二级类目路由下
    if (newVal && route.path == `/category/sub/${newVal}`) {
      // 发送请求获取数据
      filterLoading.value = true
      const { result } = await findSubCategoryFilter(newVal)
      // 给每一组可选的筛选条件缺失 ‘全部’ 条件，处理下数据加上‘全部’
      // 1. 品牌
      result.brands.selectedBrand = ref(null) // 选中的品牌 记录id
      result.brands.unshift({ id: null, name: '全部' })
      // 2，属性
      result.saleProperties.forEach((item) => {
        item.selectedAttr = ref(null) // 选中的属性 记录id
        item.properties.unshift({ id: null, name: '全部' })
      })
      filterData.value = result
      filterLoading.value = false
    }
  },
  { immediate: true }
)

// 选中的品牌
const changeBrand = (brandId) => {
  filterData.value.selectedBrand = brandId
  emit('filter-change')
}
// 选中的品牌
const changeProp = (item, propId) => {
  item.selectedAttr = propId
  emit('filter-change')
}
</script>
<style scoped lang="less">
// 骨架屏
.skl-item {
  margin: 5px;
}

// 筛选区
.sub-filter {
  background: #fff;
  padding: 25px;
  .item {
    display: flex;
    line-height: 40px;
    .head {
      width: 80px;
      color: #999;
    }
    .body {
      flex: 1;
      a {
        margin-right: 36px;
        transition: all 0.3s;
        display: inline-block;
        &.active,
        &:hover {
          color: @xtxColor;
        }
      }
    }
  }
}
</style>
