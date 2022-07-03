<template>
  <section class="payPage">
    <HeaderTop :title="'确认订单'">
      <router-link class="header_pay" slot="left"  to="/shop/goods">
        <i class="iconfont icon-jiantou2"></i>
      </router-link>
    </HeaderTop>
    <section class="pay-content">
      <div class="receive-address" @click="addAdress">
        <span><i class="iconfont icon-dingwei"></i>请添加一个收获地址</span>
        <i class="iconfont icon-jiantou1"></i>
      </div>
      <div class="receive-time">
        <span class="receive-wrapper">送达时间</span>
        <span class="arrive-wrapper">尽快送达 | 预计{{ arriveTime }}</span>
      </div>
      <div class="shopCart">
        <div class="storeName">
          <img :src="info.avatar" alt="" />
          <span>{{ info.name }}</span>
        </div>
        <ul>
          <li v-for="(item, index) of cartFoods" :key="index">
            <span class="shopName">{{ item.name }}</span>
            <span class="shopCount">x {{ item.count }}</span>
            <span class="shopPrice">￥{{ item.price * item.count }}</span>
          </li>
        </ul>
        <div class="totalPrice">
          <span>订单</span>
          <span>待支付￥{{ totalPrice }}</span>
        </div>
      </div>
    </section>
    <footer>
      <span class="totalPrice">待支付￥{{ totalPrice }}</span>
      <span class="pay-order" @click="payOrder">确认下单</span>
    </footer>
  </section>
</template>

<script>
import { mapState, mapGetters } from 'vuex'
import HeaderTop from '../../components/HeaderTop/HeaderTop.vue'
export default {
  methods: {
    addAdress() {
      console.log('添加地址')
    },
    payOrder() {
      console.log('下单')
    }
  },
  computed: {
    ...mapState(['cartFoods', 'info']),
    ...mapGetters(['totalCount', 'totalPrice']),
    // 预计到达时间，现在时间加30分钟
    arriveTime() {
      let now = new Date()
      let hour = now.getHours() + 1
      let minute = now.getMinutes()
      minute =
        Number(minute) + 30 >= 60
          ? Number(Number(minute) + 30 - 60)
          : Number(Number(minute) + 30)
      hour = minute >= 60 ? Number(Number(hour) + 1) : hour
      minute = ('0' + minute).slice(-2)
      return `${hour}:${minute}`
    }
  },
  components: {
    HeaderTop
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus" scoped>
@import '../../common/stylus/mixins.styl';

.payPage {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;

  .header_pay {
    position: absolute;
    left: 15px;
    top: 50%;
    transform: translateY(-50%);
    width: 10%;
    height: 50%;

    .icon-jiantou2 {
      font-size: 25px;
      color: #fff;
    }
  }

  .pay-content {
    position: relative;
    top: 45px;
    left: 0;
    background-color: #f5f5f5;
    height: calc(100% - 45px - 45px);

    .receive-address {
      width: 100%;
      height: 80px;
      line-height: 80px;
      background-color: #fff;
      padding-left: 15px;
      position: relative;

      .icon-dingwei {
        font-size: 28px;
        color: $green;
      }

      .icon-jiantou1 {
        position: absolute;
        right: 30px;
        font-size: 20px;
      }
    }
  }

  .receive-time {
    width: 100%;
    height: 100px;
    line-height: 100px;
    background-color: #fff;
    margin-top: 10px;
    border-left: 5px solid $green;
    position: relative;

    .receive-wrapper {
      margin-left: 15px;
      font-size: 24px;
      font-weight: 600;
    }

    .arrive-wrapper {
      position: absolute;
      top: -20px;
      right: 20px;
      display: inline;
      color: $green;
    }
  }

  .shopCart {
    width: 100%;
    height: 300px;
    background-color: #fff;
    margin-top: 10px;
    padding: 0 15px;
    position: relative;
    display: flex;
    flex-direction: column;

    .storeName {
      width: calc(100% - 30px);
      height: 50px;
      line-height: 50px;

      img {
        width: 30px;
        height: 30px;
        margin-top: 10px;
        float: left;
      }

      span {
        font-size: 20px;
        float: left;
      }
    }

    ul {
      width: calc(100% - 30px);
      flex: 1;
      overflow: auto;

      li {
        height: 40px;
        line-height: 40px;
        display: flex;

        .shopName {
          flex: 6;
        }

        .shopCount {
          flex: 1;
          color: #ff6600;
        }

        .shopPrice {
          flex: 1;
        }
      }
    }

    .totalPrice {
      width: calc(100% - 60px);
      height: 40px;
      line-height: 40px;
      top-border-1px(#f5f5f5);
      // position: absolute;
      bottom: 0;
      left: 15px;

      span {
        float: left;

        &:last-of-type {
          float: right;
          color: #ff6600;
        }
      }
    }
  }

  footer {
    height: 45px;
    line-height: 45px;
    width: 100%;
    background-color: #3c3c3c;
    color: #fff;
    position: absolute;
    bottom: 0;
    left: 0;
    display: flex;

    span {
      font-size: 20px;

      &.totalPrice {
        flex: 3;
        padding-left: 20px;
      }

      &.pay-order {
        background-color: $green;
        // padding 0 20px
        text-align: center;
        flex: 1;
      }
    }
  }
}
</style>
