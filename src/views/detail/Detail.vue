<template>
  <div id="detail">
    <detail-nav-bar class="detail-nav" @titleClick="titleClick" ref="nav"/>

    <scroll
      class="content"
      ref="scroll"
      :probe-type="3"
      @scroll="contentScroll">
      <detail-swiper :top-images="topImages" />
      <detail-base-info :goods="goods" />
      <detail-shop-info :shop-info="shopInfo" />
      <detail-info :detail-info="detailInfo" @imageLoad="imageLoad" />
      <detail-params ref="params" :param-info="paramInfo" />
      <detail-comment ref="comment" :comment-info="commentInfo" />
      <good-list ref="recommends" :goods="recommends" />
    </scroll>

    <detail-footer-bar @addCart="addToCart"/>

    <back-top @click.native="backTop" v-show="isShowBackTop"/>
  </div>
</template>

<script>
import DetailNavBar from './childComponents/DetailNavBar'
import DetailSwiper from './childComponents/DetailSwiper'
import DetailBaseInfo from './childComponents/DetailBaseInfo'
import DetailShopInfo from './childComponents/DetailShopInfo'
import DetailInfo from './childComponents/DetailInfo'
import DetailParams from './childComponents/DetailParams'
import DetailComment from './childComponents/DetailComment'
import DetailFooterBar from './childComponents/DetailFooterBar'

import Scroll from '@/components/common/scroll/Scroll'
import GoodList from '@/components/content/goods/GoodList'
import BackTop from '@/components/content/backTop/BackTop'

import { getDetail, Goods, ShopInfo,
        GoodsParam, getRecommend
} from '@/network/detail'
import { debounce } from '@/common/utils'
import { itemListenerMixin, backTopMixin } from '@/common/mixin'

import { mapActions } from 'vuex'

export default {
  data () {
    return {
      iid: null,
      topImages: [],
      goods: {},
      shopInfo: {},
      detailInfo: {},
      paramInfo: {},
      commentInfo: {},
      recommends: [],
      themeTopYs: [],
      getThemeTopY: null,
      currentIndex: 0
    }
  },
  mixins: [itemListenerMixin, backTopMixin],
  components: {
    DetailNavBar,
    DetailSwiper,
    DetailBaseInfo,
    DetailShopInfo,
    DetailInfo,
    DetailParams,
    DetailComment,
    DetailFooterBar,
    Scroll,
    GoodList
  },
  created() {
    // 1. 保存传入的iid
    this.iid = this.$route.params.iid

    // 根据iid请求详情数据
    getDetail(this.iid).then(res => {
      // 获取顶部的图片轮播数据
      const data = res.result
      this.topImages = data.itemInfo.topImages

      // 获取商品信息
      this.goods = new Goods(data.itemInfo, data.columns, data.shopInfo.services);

      // 获取店铺信息
      this.shopInfo = new ShopInfo(data.shopInfo);

      // 保存商品的详情数据
      this.detailInfo = data.detailInfo

      // 获取参数信息
      this.paramInfo = new GoodsParam(data.itemParams.info, data.itemParams.rule)

      // 取出评论的信息
      if(data.rate.cRate !== 0) {
        this.commentInfo = data.rate.list[0]
      }

      // 请求推荐数据
      getRecommend().then(res => {
        this.recommends = res.data.list
      })

      // this.$nextTick(() => {
      //   // 根据最新的数据, 对应的DOM是已经被渲染出来了
      //   // 但是图片依然是没有加载完(目前获取到的offsetTop不包含其中的图片)
      //   // offsetTop值不对的时候, 都是因为图片的问题
      //   this.themeTopYs = []
      //   this.themeTopYs.push(0);
      //   this.themeTopYs.push(this.$refs.params.$el.offsetTop);
      //   this.themeTopYs.push(this.$refs.comment.$el.offsetTop);
      //   this.themeTopYs.push(this.$refs.recommends.$el.offsetTop);
      //   console.log(this.themeTopYs)
      // })

      // 给getThemeTopY赋值(对this.themeTopYs赋值的操作进行防抖)
      this.getThemeTopY = debounce(() => {
        this.themeTopYs = []
        this.themeTopYs.push(0);
        this.themeTopYs.push(this.$refs.params.$el.offsetTop);
        this.themeTopYs.push(this.$refs.comment.$el.offsetTop);
        this.themeTopYs.push(this.$refs.recommends.$el.offsetTop);
        this.themeTopYs.push(Number.MAX_VALUE);
      }, 100)

    })
  },
  methods: {
    ...mapActions(['addCart']),
    imageLoad() {
      // this.$refs.scroll.refresh()
      this.newRefresh()
      this.getThemeTopY()
    },
    titleClick(index) {
      this.$refs.scroll.scrollTo(0, -this.themeTopYs[index], 100)
    },
    contentScroll(position) {
      // 1.获取y值
      const positionY = -position.y

      // 2.positionY和主题中的值进行对比
      // for(let i in this.themeTopYs) {

      //   if(positionY > this.themeTopYs[i] && positionY < this.themeTopYs[i+1]) {
      //     console.log(i)
      //   }
      // }
      let length = this.themeTopYs.length
      for(let i = 0; i < length - 1; i++) {
        // if(this.currentIndex !== i && (i < length - 1 && positionY >= this.themeTopYs[i] &&
        //   positionY <= this.themeTopYs[i+1]) ||
        //   (i === length - 1 && positionY >= this.themeTopYs[i])) {
          // this.currentIndex = i;
          // console.log(this.currentIndex)
          // this.$refs.nav.currentIndex = this.currentIndex
        // }

        if(this.currentIndex !== i && (positionY >= this.themeTopYs[i]
        && positionY <= this.themeTopYs[i+1])) {
          this.currentIndex = i;
          this.$refs.nav.currentIndex = this.currentIndex
        }
      }

      // 回到顶部
      this.isShowBackTop = (-position.y) > 1000
    },
    addToCart() {
      // 1. 获取购物车需要展示的信息
      const product = {}
      product.image = this.topImages[0];
      product.title = this.goods.title;
      product.desc = this.goods.desc;
      product.price = this.goods.realPrice;
      product.iid = this.iid;

      // 2.将商品添加到购物车
      // this.$store.commit('addCart', product)
      // this.$store.dispatch('addCart', product).then(res => {
      //   console.log(res)
      // })
      this.addCart(product).then(res => {
        this.$toast.show(res, 20000);
      })
    }
  },
  mounted() {
  },
  destroyed() {
    this.$bus.$off('itemImageLoad', this.itemListener)
  }
}
</script>

<style scoped>
#detail {
  position: relative;
  z-index: 90;
  background: #ffffff;
  height: 100vh;
}

.detail-nav {
  position: relative;
  z-index: 99;
  background: #ffffff;
}

.content {
  height: calc(100% - 44px - 49px);
}
</style>
