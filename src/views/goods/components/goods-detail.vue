<template>
  <div class="goods-detail">
    <div class="goods-detail">
      <!-- 属性 -->
      <ul class="attrs">
        <li v-for="item in goods.details.properties" :key="item.value">
          <span class="dt">{{ item.name }}</span>
          <span class="dd">{{ item.value }}</span>
        </li>
      </ul>
      <!-- 图片 -->
      <img
        v-for="item in goods.details.pictures"
        :key="item"
        :src="item"
        alt=""
      />
    </div>
  </div>
</template>

<script setup>
import { inject, ref } from 'vue'
const goods = inject('goods')
</script>

<style scoped lang="less">
.goods-detail {
  padding: 40px;
  .attrs {
    display: flex;
    flex-wrap: wrap;
    margin-bottom: 30px;
    li {
      display: flex;
      margin-bottom: 10px;
      width: 50%;
      .dt {
        width: 100px;
        color: #999;
      }
      .dd {
        flex: 1;
        color: #666;
      }
    }
  }
  > img {
    width: 100%;
  }
}
</style>
