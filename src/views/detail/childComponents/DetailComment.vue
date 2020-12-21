<template>
  <div v-if="Object.keys(commentInfo).length !== 0" class="comment-info">
    <div class="info-header">
      <div class="header-title">用户评价</div>
      <div class="header-more">
        更多
        <i class="arrow-right"></i>
      </div>
    </div>
    <div class="info-user">
      <img :src="commentInfo.user.avatar">
      <span>{{ commentInfo.user.uname }}</span>
    </div>
    <div class="info-content">{{ commentInfo.content }}</div>
    <div class="info-detail">
      <div class="info-other">
        <span class="date">{{ commentInfo.created }}</span>
        <span>{{ commentInfo.style }}</span>
      </div>
      <div class="info-img">
        <img v-for="(item, index) in commentInfo.images"
            :key="index" :src="item" alt="">
      </div>
    </div>
  </div>
</template>

<script>
  import { dateFormat } from '@/common/utils'

  export default {
    props: {
      commentInfo: {
        type: Object,
        default() {
          return {}
        }
      }
    },
    filters: {
      showDate(value) {
        // const date = new Date(value * 1000)
        // return dateFormat(date, 'yyyy-MM-dd')
      }
    }
  }
</script>

<style scoped>
  .comment-info {
    padding: 5px 12px;
    color: #333;
    border-bottom: 5px solid #f2f5f8;
  }
  .info-header {
    height: 50px;
    line-height: 50px;
    display: flex;
    justify-content: space-between;
    border-bottom: 1px solid rgba(0, 0, 0, .1);
  }
  .info-user {
    margin-top: 10px;
    width: 100vw;
    line-height: 45px;
    height: 45px;
    font-weight: bold;
    position: relative;
  }
  .info-user img {
    width: 45px;
    height: 45px;
    border-radius: 50%;
  }
  .info-user span {
    position: absolute;
    margin-left: 10px;
  }
  .info-content {
    font-size: 14px;
    margin-top: 10px;
  }
  .info-img {
    display: flex;
    padding: 0 3px;
  }
  .info-img img {
    width: 60px;
    height: 80px;
  }
  .info-detail {
    font-size: 12px;
    color: gray;
    margin-top: 5px;
  }
  .date {
    margin-right: 10px;
    margin-bottom: 8px;
  }
</style>
