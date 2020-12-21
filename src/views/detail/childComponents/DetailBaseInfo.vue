<template>
  <div v-if="Object.keys(goods).length !== 0" class="base-info">
    <div class="title">{{ goods.title }}</div>
    <div class="price">
      <span class="newPrice">{{ goods.newPrice }}</span>
      <span class="oldPrice">{{ goods.oldPrice }}</span>
      <span class="discount">{{ goods.discount }}</span>
    </div>
    <div class="columns">
      <span v-for="(item, index) in goods.columns"
            :key="index">{{item}}</span>
    </div>
    <div class="services">
      <span v-for="index in goods.services.length -1">
        <img :src="goods.services[index-1].icon" alt="">
        <span>{{ goods.services[index - 1].name }}</span>
      </span>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    goods: {
      type: Object,
      default() {
        return {}
      }
    }
  }
}
</script>

<style scoped>
  .base-info {
    margin-top: 8px;
  }

  .title {
    font-size: 14px;
    font-weight: bold;
    text-indent: 0.5rem;
  }

  .price {
    display: flex;
    margin-top: 5px;
  }
  .newPrice {
    color: var(--color-high-text);
    font-size: 16px;
  }
  .oldPrice {
    font-size: 11px;
    color: gray;
    text-decoration: line-through;
    margin-top: 2px;
    margin-left: 5px;
  }
  .discount {
    text-align: center;
    background: var(--color-high-text);
    border-radius: 8px;
    padding-right: 10px;
    padding-top: 2px;
    margin-left: 15px;
    text-align: center;
  }

  .columns {
    display: flex;
    font-size: 12px;
    color: gray;
    margin-top: 15px;
    justify-content: space-between;
  }

  .services {
    margin-top: 25px;
    display: flex;
    font-size: 12px;
    justify-content: space-between;
  }
  .services img {
    width: 10px;
    height: 10px;
  }
</style>
