<template>
  <div class="goods-image">
    <!-- 大图放大镜 -->
    <div
      class="large"
      v-show="showLayer"
      :style="[{ backgroundImage: `url(${images[currIndex]})` }, largePosition]"
    ></div>
    <!-- 中图 -->
    <div class="middle" ref="target">
      <img :src="images[currIndex]" alt="" />
      <!-- 遮罩块 -->
      <div class="layer" v-show="showLayer" :style="layerPosition"></div>
    </div>

    <!-- 图片右边的五个略缩图 -->
    <ul class="small">
      <li
        v-for="(img, i) in images"
        :key="img"
        :class="{ active: i === currIndex }"
      >
        <img @mouseenter="currIndex = i" :src="img" alt="" />
      </li>
    </ul>
  </div>
</template>
<script setup>
import { reactive, ref, toRefs, watch, watchEffect } from 'vue'
import { useMouseInElement } from '@vueuse/core'
defineProps({
  images: {
    type: Array,
    default: () => [],
  },
})
const target = ref(null)
// 当前预览图的索引
const currIndex = ref(0)

// 1 是否显示遮罩层和大图
const showLayer = ref(false)
// 2 遮罩的坐标(样式)
const layerPosition = reactive({
  left: 0,
  top: 0,
})
// 3 大图的背景定位(样式)
const largePosition = reactive({
  backgroundPositionX: 0,
  backgroundPositionY: 0,
})

// 获取坐标
const { isOutside, elementX, elementY } = useMouseInElement(target)
watch([isOutside, elementX, elementY], () => {
  // 是否显示遮罩层
  showLayer.value = !isOutside.value
  const position = { x: 0, y: 0 }

  // 限定遮罩层移动的范围 中间区域
  if (elementX.value < 100) position.x = 0
  else if (elementX.value > 300) position.x = 200
  else position.x = elementX.value - 100

  if (elementY.value < 100) position.y = 0
  else if (elementY.value > 300) position.y = 200
  else position.y = elementY.value - 100

  // 设置值
  layerPosition.left = `${position.x}px`
  layerPosition.top = `${position.y}px`
  largePosition.backgroundPositionX = `${-2 * position.x}px`
  largePosition.backgroundPositionY = `${-2 * position.y}px`
})
</script>

<style scoped lang="less">
.goods-image {
  width: 480px;
  height: 400px;
  position: relative;
  display: flex;
  z-index: 500;
  .large {
    position: absolute;
    top: 0;
    left: 412px;
    width: 400px;
    height: 400px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    background-repeat: no-repeat;
    background-size: 800px 800px;
    background-color: #f8f8f8;
  }
  .middle {
    width: 400px;
    height: 400px;
    background: #f5f5f5;
    position: relative;
    cursor: move;
    .layer {
      width: 200px;
      height: 200px;
      background: rgba(0, 0, 0, 0.2);
      left: 0;
      top: 0;
      position: absolute;
    }
  }
  .small {
    width: 80px;
    li {
      width: 68px;
      height: 68px;
      margin-left: 12px;
      margin-bottom: 15px;
      cursor: pointer;
      &:hover,
      &.active {
        border: 2px solid @xtxColor;
      }
    }
  }
}
</style>
