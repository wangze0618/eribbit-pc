<template>
  <transition name="down">
    <div class="xtx-message" v-if="isShow" :style="style[type]">
      <!-- 上面绑定的是样式 -->
      <!-- 不同提示图标会变 -->
      <i class="iconfont" :class="[style[type].icon]"></i>
      <span class="text">{{ text }}</span>
    </div>
  </transition>
</template>
<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
const props = defineProps({
  text: {
    type: String,
    default: '',
  },
  type: {
    type: String,
    // warn 警告  error 错误  success 成功
    default: 'warn',
  },
})
const isShow = ref(false)
onMounted(async () => {
  isShow.value = true
})

// 定义一个对象，包含三种情况的样式，对象key就是类型字符串
const style = {
  warn: {
    icon: 'icon-warning',
    color: '#E6A23C',
    backgroundColor: 'rgb(253, 246, 236)',
    borderColor: 'rgb(250, 236, 216)',
  },
  error: {
    icon: 'icon-shanchu',
    color: '#F56C6C',
    backgroundColor: 'rgb(254, 240, 240)',
    borderColor: 'rgb(253, 226, 226)',
  },
  success: {
    icon: 'icon-queren2',
    color: '#67C23A',
    backgroundColor: 'rgb(240, 249, 235)',
    borderColor: 'rgb(225, 243, 216)',
  },
}
</script>
<style scoped lang="less">
.down-enter-active {
  transition: all 0.5s ease-out;
}
.down-leave-active {
  // transition: all 0.5s;
  transition: all 0.5s cubic-bezier(1, 0.5, 0.8, 1);
}
.down-enter-from,
.down-leave-to {
  transform: translateY(-75px);
  opacity: 0;
}
// .down-enter-to,
// .down-leave-from {
//   transform: none;
//   opacity: 1;
// }
.xtx-message {
  width: 300px;
  height: 50px;
  position: fixed;
  z-index: 9999;
  left: 50%;
  margin-left: -150px;
  top: 25px;
  line-height: 50px;
  padding: 0 25px;
  border: 1px solid #e4e4e4;
  background: #f5f5f5;
  color: #999;
  border-radius: 4px;
  i {
    margin-right: 4px;
    vertical-align: middle;
  }
  .text {
    vertical-align: middle;
  }
}
</style>
