<template>
  <div class="xtx-member-aside d-none d-lg-block">
    <div class="user-manage">
      <h4>我的账户</h4>
      <div class="links">
        <router-link exact-active-class="active" to="/member"
          >个人中心</router-link
        >
        <a href="javascript:;">消息通知</a>
        <router-link exact-active-class="active" to="/member/info"
          >个人信息</router-link
        >
        <a href="javascript:;">安全设置</a>
        <a href="javascript:;">地址管理</a>
        <a href="javascript:;">我的积分</a>
        <a href="javascript:;">我的足迹</a>
        <a href="javascript:;">邀请有礼</a>
        <a href="javascript:;">幸运抽奖</a>
      </div>
      <h4>交易管理</h4>
      <div class="links">
        <router-link
          active-class="active"
          to="/member/order/"
          href="javascript:;"
          >我的订单</router-link
        >
        <a href="javascript:;">优惠券</a>
        <a href="javascript:;">礼品卡</a>
        <a href="javascript:;">评价晒单</a>
        <a href="javascript:;">售后服务</a>
      </div>
      <h4>我的收藏</h4>
      <div class="links">
        <a href="javascript:;">收藏的商品</a>
        <a href="javascript:;">收藏的专题</a>
        <a href="javascript:;">关注的品牌</a>
      </div>
      <h4>帮助中心</h4>
      <div class="links">
        <a href="javascript:;">帮助中心</a>
        <a href="javascript:;">在线客服</a>
      </div>
    </div>
  </div>
</template>

<script setup></script>

<style scoped lang="less">
.active {
  color: @xtxColor !important;
  &::after {
    // display: flex;
    float: left;
    content: '';
    height: 8px;
    width: 8px;
    margin-left: -18px;
    border-radius: 4px;
    margin-top: 3px;
    background-color: @xtxColor;
  }
}
.xtx-member-aside {
  min-width: 200px;
  margin-right: 10px;
  border-radius: 2px;
  .user-manage {
    background-color: #fff;
    h4 {
      font-size: 18px;
      font-weight: 400;
      padding: 20px 52px 5px;
      border-top: 1px solid #f6f6f6;
    }

    .links {
      padding: 0 52px 10px;
    }

    a {
      display: block;
      line-height: 1;
      padding: 15px 0;
      font-size: 14px;
      color: #666;
      position: relative;

      &:hover {
        color: @xtxColor;
      }
      &.active {
        color: @xtxColor;

        &:before {
          display: block;
        }
      }

      &:before {
        content: '';
        display: none;
        width: 6px;
        height: 6px;
        border-radius: 50%;
        position: absolute;
        top: 19px;
        left: -16px;
        background-color: @xtxColor;
      }
    }
  }
}
</style>
