<template>
  <div class="detail-info" v-if="Object.keys(detailInfo).length !== 0">
    <div class="title">
      <div class="start">
        <div class="spot"></div>
        <div class="line"></div>
      </div>
      <span class="desc">{{ detailInfo.desc }}</span>
      <div class="end">
        <div class="spot"></div>
        <div class="line"></div>
      </div>
    </div>
    <div class="look">{{ detailInfo.detailImage[0].key }}</div>
    <div class="detail-img">
      <img
        src=""
        v-for="(item, index) in detailInfo.detailImage[0].list"
        :key="index"
        :src="item"
        @load="imageLoad"
      />
    </div>
  </div>
</template>

<script>
export default {
  props: {
    detailInfo: {
      type: Object,
      default() {
        return {};
      }
    }
  },
  data() {
    return {
      counter: 0,
      imageLength: 0
    };
  },
  methods: {
    imageLoad() {
      if (++this.counter === this.imageLength) {
        this.$emit("imageLoad");
      }
    }
  },
  watch: {
    detailInfo() {
      // 获取图片个数
      this.imageLength = this.detailInfo.detailImage[0].list.length;
    }
  }
};
</script>

<style scoped>
.desc {
  font-size: 12px;
}
.start .spot {
  width: 0.04rem;
  height: 0.04rem;
  border: 1px solid #000000;
  background-color: #000000;
}
.start .line {
  width: 1rem;
  height: 1px;
  background-color: #d5d5d5;
}
.end {
  display: flex;
  flex-direction: column;
  width: 1rem;
  right: 0px;
}
.end .spot {
  align-self: flex-end;
  width: 0.04rem;
  height: 0.04rem;
  background-color: #000000;
  border: 1px solid #000000;
}
.end .line {
  width: 1rem;
  height: 1px;
  text-align: right;
  background-color: #d5d5d5;
}
.look {
  font-size: 15px;
  font-weight: bold;
  margin-top: 8px;
  margin-left: 20px;
}
img {
  width: 100vw;
}
</style>
