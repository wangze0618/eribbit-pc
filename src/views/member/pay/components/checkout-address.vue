<template>
  <div class="checkout-address">
    <div class="text">
      <div v-if="!showAddress" class="none">
        您需要先添加收货地址才可提交订单。
      </div>
      <ul v-if="showAddress">
        <li>
          <span>收<i />货<i />人：</span>{{ showAddress.receiver }}
        </li>
        <li><span>联系方式：</span>{{ hidePhone(showAddress.contact) }}</li>
        <li>
          <span>收货地址：</span>{{ showAddress.fullLocation
          }}{{ showAddress.address }}
        </li>
      </ul>
      <a
        v-if="showAddress"
        @click="openAddressEdit(showAddress)"
        href="javascript:;"
        >修改地址</a
      >
    </div>
    <div class="action">
      <XtxButton @click="openDialog()" class="btn">切换地址</XtxButton>
      <XtxButton @click="openAddressEdit({})" class="btn">添加地址</XtxButton>
    </div>
  </div>
  <!-- 对话框组件 -->
  <XtxDialog title="切换收货地址" v-model:visible="showDialog">
    <template #default>
      <div
        @click="selectedAddress = item"
        class="text item dialogBody"
        :class="{ active: selectedAddress.id == item.id }"
        v-for="item in list"
        :key="item.id"
      >
        <ul>
          <li>
            <span>收<i />货<i />人：</span>{{ item.receiver }}
          </li>
          <li><span>联系方式：</span>{{ hidePhone(item.contact) }}</li>
          <li>
            <span>收货地址：</span
            >{{ item.fullLocation.replace(/ /g, '') + item.address }}
          </li>
        </ul>
      </div>
    </template>
    <template #footer>
      <XtxButton
        @click="showDialog = false"
        type="gray"
        style="margin-right: 20px"
        >取消</XtxButton
      >
      <XtxButton @click="confirmAddress()" type="primary">确认</XtxButton>
    </template>
  </XtxDialog>

  <!--  收货地址添加编辑组件 -->
  <AddressEdit ref="addressEditCom" @on-success="successFn($event)" />
</template>

<script setup>
import { nextTick, onMounted, reactive, ref } from 'vue'
import XtxButton from '@/components/library/xtx-button.vue'
import XtxDialog from '@/components/library/xtx-dialog.vue'
import AddressEdit from './address-edit.vue'
import { getAddress } from '@/api/order'
const props = defineProps({
  list: {
    // 收货地址列表
    type: Array,
    default: () => [],
  },
})
const emit = defineEmits(['change'])
/*
1. 找到默认收货地址
2. 没有默认地址，就使用第一条收货地址
3. 如没有数据，就提示添加地址
*/
const addressList = props.list
const showAddress = ref(null)
const defaultAddress = addressList.find((item) => item.isDefault === 0)
if (defaultAddress) {
  showAddress.value = defaultAddress
} else {
  if (props.list.length) {
    showAddress.value = addressList[0]
  }
}
const showDialog = ref(false)

// 切换地址
const openDialog = () => {
  showDialog.value = true
}

const successFn = (data) => {
  const address = props.list.find((item) => item.id === data.id)
  if (address) {
    for (const key in address) {
      address[key] = data[key]
    }
  } else {
    const dataObj = JSON.stringify(data)
    props.list.unshift(JSON.parse(dataObj))
  }
}

// 添加编辑收货地址组件
const addressEditCom = ref(null)
const openAddressEdit = (address) => {
  addressEditCom.value.open(address)
}
// 通知父组件-默认地址的id
emit('change', showAddress.value?.id)

// 记录当前选中的地址Id
const selectedAddress = ref('null')

// 确认地址
const confirmAddress = () => {
  try {
    showAddress.value = selectedAddress.value
    emit('change', selectedAddress.value?.id)
    showDialog.value = false
  } catch (error) {}
}

// 电话号码部分隐藏
const hidePhone = (phone) => {
  if (phone) {
    return phone.substring(0, 3) + '****' + phone.substr(phone.length - 4)
  }
}
</script>
<style scoped lang="less">
.dialogBody {
  // &.item {
  border: 1px solid #f5f5f5;
  margin-bottom: 10px;
  cursor: pointer;
  &.active,
  &:hover {
    border-color: @xtxColor;
    background: lighten(@xtxColor, 50%);
  }
  > ul {
    padding: 10px;
    font-size: 14px;
    line-height: 30px;
  }
  // }
}
.checkout-address {
  border: 1px solid #f5f5f5;
  display: flex;
  align-items: center;
  .text {
    flex: 1;
    min-height: 90px;
    display: flex;
    align-items: center;
    .none {
      line-height: 90px;
      color: #999;
      text-align: center;
      width: 100%;
    }
    > ul {
      flex: 1;
      padding: 20px;
      li {
        line-height: 30px;
        span {
          color: #999;
          margin-right: 5px;
          > i {
            width: 0.5em;
            display: inline-block;
          }
        }
      }
    }
    > a {
      color: @xtxColor;
      width: 160px;
      text-align: center;
      height: 90px;
      line-height: 90px;
      border-right: 1px solid #f5f5f5;
    }
  }
  .action {
    width: 420px;
    text-align: center;
    .btn {
      width: 140px;
      height: 46px;
      font-size: 14px;
      &:first-child {
        margin-right: 10px;
      }
    }
  }
}
</style>
