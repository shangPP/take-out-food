<template>
  <section class="msite">
    <!--首页头部-->
    <HeaderTop :title="address.name">
      <!--      msite头部左边搜索图标-->
      <router-link class="header_search" slot="left" to="/search">
        <i class="iconfont icon-sousuo"></i>
      </router-link>
      <router-link
        class="header_login"
        slot="right"
        :to="userInfo._id ? '/userinfo' : '/login'"
      >
        <!--        未登录时出现的文本内容-->
        <span class="header_login_text" v-if="!userInfo._id"> 登录|注册 </span>
        <!--          登陆成功后的人物图标-->
        <span class="header_login_text" v-else>
          <i class="iconfont icon-person"></i>
        </span>
      </router-link>
    </HeaderTop>

    <div class="miste-content-wrapper">
      <div class="miste-content">
        <!--首页导航：轮播图-->
        <nav class="msite_nav">
          <!--          categorys找不到，因此显示不出来-->
          <div class="swiper-container" v-if="categorys.length">
            <div class="swiper-wrapper">
              <div
                class="swiper-slide"
                v-for="(categorys, index) in categorysArr"
                :key="index"
              >
                <a
                  href="javascript:"
                  class="link_to_food"
                  v-for="(category, index2) in categorys"
                  :key="index2"
                  @click="swiperClick(category)"
                >
                  <div class="food_container">
                    <img :src="baseImageUrl + category.image_url" />
                  </div>
                  <span>{{ category.title }}</span>
                </a>
              </div>
            </div>
            <!-- Add Pagination -->
            <div class="swiper-pagination"></div>
          </div>
          <img src="./images/msite_back.svg" alt="back" v-else />
        </nav>
        <!--首页附近商家-->
        <div class="msite_shop_list">
          <div class="shop_header">
            <i class="iconfont icon-dianpu"></i>
            <span class="shop_header_title">附近商家</span>
          </div>
          <ShopList />
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import BScroll from '@better-scroll/core'
import { mapState } from 'vuex'
import Swiper from 'swiper'
import 'swiper/dist/css/swiper.min.css'
import { Toast } from 'mint-ui'

import HeaderTop from '../../components/HeaderTop/HeaderTop.vue'
import ShopList from '../../components/ShopList/ShopList.vue'

export default {
  data() {
    return {
      baseImageUrl: 'https://fuss10.elemecdn.com'
    }
  },
  mounted() {
    // 改变state，异步操作
    this.$store.dispatch('getCategorys')
    this.$store.dispatch('getShops')
  },
  methods: {
    swiperClick(category) {
      Toast(category.title + '~待开发中!!!')
    }
  },
  computed: {
    // 当一个组件需要获取多个状态的时候，将这些状态都声明为计算属性会有些重复和冗余。为了解决这个问题，我们可以使用 mapState 辅助函数帮助我们生成计算属性
    // mapState 函数返回的是一个对象
    ...mapState(['address', 'categorys', 'userInfo']),

    /*
     * 根据categorys一维数组生成一个二维数组
     * 小数组中的元素个数最大是8
     * */
    categorysArr() {
      const { categorys } = this
      //  准备空的二维数组
      const arr = []
      // 准备一个小数组(最大长度是8)
      let minArr = []
      // 遍历categorys
      if (categorys) {
        categorys.forEach(c => {
          // 如果当前小数组已经满了，创建一个新的
          if (minArr.length === 8) {
            minArr = []
          }
          // 如果minArr是空的，将小数组保存到大数组中
          if (minArr.length === 0) {
            arr.push(minArr)
          }
          // 将当前分类保存到小数组中
          minArr.push(c)
        })
        return arr
      }
    }
  },
  watch: {
    categorys(value) {
      // categorys数组中有数据了，在异步更新界面之前执行
      // 使用setTimeout可以实现效果，但不是太好
      // setTimeout(() => {
      //   //  创建一个swiper实例对象，来实现轮播
      //   /* eslint-disable no-new */
      //   new Swiper('.swiper-container', {
      //     loop: true, // 可以循环轮播
      //     // 如果需要分页器
      //     pagination: {
      //       el: '.swiper-pagination'
      //     }
      //   })
      // }, 100)

      // 界面更新就立即创建swiper对象
      this.$nextTick(() => {
        // 一旦完成界面更新，立即调用（此条语句要写在数据更新之后）
        //  创建一个swiper实例对象，来实现轮播
        /* eslint-disable no-new */
        new Swiper('.swiper-container', {
          loop: true, // 可以循环轮播
          // 如果需要分页器
          pagination: {
            el: '.swiper-pagination'
          }
        })
        new BScroll('.miste-content-wrapper', {
          click: true
        })
      })
    }
  },
  components: {
    HeaderTop,
    ShopList
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import '../../common/stylus/mixins.styl';

.msite { // 首页
  width: 100%;

  .header_search {
    position: absolute;
    left: 15px;
    top: 50%;
    transform: translateY(-50%);
    width: 10%;
    height: 50%;

    .icon-sousuo {
      font-size: 25px;
      color: #fff;
    }
  }

  .miste-content-wrapper {
    position: fixed;
    top: 45px;
    bottom: 50px;
    width: 100%;

    .miste-content {
      background: #f5f5f5;

      .msite_nav {
        bottom-border-1px(#e4e4e4);
        // margin-top 45px
        height: 200px;
        background: #fff;

        .swiper-container {
          width: 100%;
          height: 100%;

          .swiper-wrapper {
            width: 100%;
            height: 100%;

            .swiper-slide {
              display: flex;
              justify-content: center;
              align-items: flex-start;
              flex-wrap: wrap;

              .link_to_food {
                width: 25%;

                .food_container {
                  display: block;
                  width: 100%;
                  text-align: center;
                  padding-bottom: 10px;
                  font-size: 0;

                  img {
                    display: inline-block;
                    width: 50px;
                    height: 50px;
                  }
                }

                span {
                  display: block;
                  width: 100%;
                  text-align: center;
                  font-size: 13px;
                  color: #666;
                }
              }
            }
          }

          .swiper-pagination {
            >span.swiper-pagination-bullet-active {
              background: #02a774;
            }
          }
        }
      }

      .msite_shop_list {
        top-border-1px(#e4e4e4);
        margin-top: 10px;
        background: #fff;

        .shop_header {
          padding: 10px 10px 0;

          .shop_icon {
            margin-left: 5px;
            color: #999;
          }

          .shop_header_title {
            color: #999;
            font-size: 14px;
            line-height: 20px;
          }
        }
      }
    }
  }
}
</style>
