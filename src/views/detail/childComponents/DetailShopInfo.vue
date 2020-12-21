<template>
  <div class="shop-info">
    <div class="shop">
      <img :src="shopInfo.logo" />
      <span>{{ shopInfo.name }}</span>
    </div>
    <div class="left">
      <div class="total">
        <span>{{ shopInfo.sells }}</span>
        <p>总数量</p>
      </div>
      <div class="baby">
        <span>{{ shopInfo.fans }}</span>
        <p>全部宝贝</p>
      </div>
    </div>
    <div class="right">
      <div class="score" v-for="(item, index) in shopInfo.score" :key="index">
        <span>{{ item.name }}</span>
        <span class="score-item">{{ item.score }}</span>
        <span class="better" :class="{ better_bg: item.isBetter }">{{
          item.isBetter ? "高" : "低"
        }}</span>
      </div>
    </div>
    <div class="btn">
      <button>进店逛逛</button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    shopInfo: {
      type: Object,
      default() {
        return {};
      }
    }
  },
  data() {
    return {
      isBetter: true
    };
  }
};
</script>

<style scoped>
.shop-info {
  margin-top: 25px;
}
.shop {
  height: 50px;
  padding: 10px;
  line-height: 50px;
}
.shop img {
  width: 50px;
  height: 50px;
}
.shop span {
  display: inline-block;
  margin-left: 20px;
  position: absolute;
}
.left {
  width: 50%;
  margin-top: 30px;
  text-align: center;
  display: flex;
  float: left;
  justify-content: center;
  padding: 15px 0;
}
.total p,
.baby p {
  margin-top: 8px;
}
.right {
  margin-top: 20px;
  width: 50%;
}
.score {
  padding: 8px 0;
}
.score-item {
  margin-left: 15px;
}

.better {
  background-color: red;
  color: white;
  padding: 0 3px;
  margin-left: 8px;
  float: right;
  margin-right: 10px;
}

.better_bg {
  background-color: #67914b !important;
}
.btn {
  width: 100vw;
  height: 200px;
  border-bottom: 8px solid #eeeeee;
  margin-bottom: 25px;
}
button {
  width: 100px;
  height: 40px;
  margin: 0 auto;
  background: #eeeeee;
  border: none;
  margin-left: 105px;
  margin-top: 15px;
}
</style>
