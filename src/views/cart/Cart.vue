<template>
  <div class="cart">
    <nav-bar class="nav-bar">
      <div slot="center">购物车({{ length }})</div>
    </nav-bar>

    <!-- 商品列表 -->
    <cart-list/>

    <!-- 成品汇总 -->
    <cart-bottom-bar/>
  </div>
</template>

<script>
  import NavBar from '@/components/common/navbar/NavBar'
  import CartList from './chilidComps/CartList'
  import CartBottomBar from './chilidComps/CartBottomBar'

  import { mapGetters } from 'vuex'

  export default {
    components: {
      NavBar,
      CartList,
      CartBottomBar
    },
    computed: {
      // ...mapGetters(['cartLength'])
      ...mapGetters({
        length: 'cartLength'
      })
    }
  }
</script>

<style scoped>
  .cart {
    height: 100vh;
  }
  .nav-bar {
    background: var(--color-tint);
    color: #ffffff;
  }
</style>
