<template>
  <div class="xtx-numbox">
    <div class="label" v-if="label">{{ label }}</div>

    <div class="numbox">
      <a href="javascript:;" @click="changeNum(-1)">-</a>
      <input type="text" readonly :value="count" />
      <a href="javascript:;" @click="changeNum(+1)">+</a>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
const props = defineProps({
  count: {
    type: Number,
    required: true,
    default: 1,
  },
  min: {
    type: Number,
    default: 1,
  },
  max: {
    type: Number,
    default: 10,
  },
  label: {
    type: String,
    default: '',
  },
})
let newVal = props.count
const emit = defineEmits(['change'])

// 数量改变
const changeNum = (num) => {
  newVal += num
  if (newVal < props.min || newVal > props.max) return
  // 通知父组件
  emit('change', newVal)
}
</script>

<style scoped lang="less">
.xtx-numbox {
  display: flex;
  align-items: center;
  .label {
    width: 60px;
    color: #999;
    padding-left: 10px;
  }
  .numbox {
    width: 120px;
    height: 30px;
    border: 1px solid #e4e4e4;
    display: flex;
    > a {
      width: 29px;
      line-height: 28px;
      text-align: center;
      background: #f8f8f8;
      font-size: 16px;
      color: #666;
      &:first-of-type {
        border-right: 1px solid #e4e4e4;
      }
      &:last-of-type {
        border-left: 1px solid #e4e4e4;
      }
    }
    > input {
      border: unset;
      width: 60px;
      padding: 0 5px;
      text-align: center;
      color: #666;
      &:focus {
        outline: none;
      }
    }
  }
}
</style>
