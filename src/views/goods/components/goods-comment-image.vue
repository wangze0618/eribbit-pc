<template>
  <div class="goods-comment-image">
    <div class="list">
      <a
        @click="currImage = url"
        href="javascript:;"
        v-for="url in pictures"
        :key="url"
        :class="{ active: url == currImage }"
      >
        <img alt="" :src="url" />
      </a>
    </div>
    <div class="preview" v-if="currImage">
      <img :src="currImage" alt="" />
      <i @click="currImage = null" class="iconfont icon-close-new"></i>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
const props = defineProps({
  pictures: {
    type: Array,
    default: () => [],
  },
})
const currImage = ref(null)
</script>
<style scoped lang="less">
.goods-comment-image {
  .list {
    display: flex;
    flex-wrap: wrap;
    margin-top: 10px;
    a {
      width: 120px;
      height: 120px;
      border: 1px solid #e4e4e4;
      margin-right: 20px;
      margin-bottom: 10px;
      img {
        width: 100%;
        height: 100%;
        object-fit: contain;
      }
      &.active {
        border-color: @xtxColor;
      }
    }
  }
}
.preview {
  width: 480px;
  height: 480px;
  border: 1px solid #e4e4e4;
  background: #f8f8f8;
  margin-bottom: 20px;
  position: relative;
  img {
    width: 100%;
    height: 100%;
    object-fit: contain;
  }
  i {
    position: absolute;
    right: 0;
    top: 0;
    width: 30px;
    height: 30px;
    background: rgba(0, 0, 0, 0.2);
    color: #fff;
    text-align: center;
    line-height: 30px;
    &:hover {
      background-color: rgb(255, 0, 0);
    }
  }
}
</style>
