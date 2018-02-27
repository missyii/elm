<template>
<div class="ratings" ref="rat">
  <div class="ratings-content">
    <div class="overview">
      <div class="overview-left">
        <h1 class="score">{{seller.score}}</h1>
        <div class="title">综合评分</div>
        <div class="rank">高于周边商家{{seller.rankRate}}%</div>
      </div>
      <div class="overview-right">
        <div class="score-wrapper">
          <span class="title">服务态度</span>
          <star :size="36" :score="seller.serviceScore" class="star"></star>
          <span class="score">{{seller.serviceScore}}</span>
        </div>
        <div class="score-wrapper">
          <span class="title">商品评分</span>
          <star :size="36" :score="seller.foodScore" class="star"></star>
          <span class="score">{{seller.foodScore}}</span>
        </div>
        <div class="time-wrapper">
          <span class="title">送达时间</span>
          <span class="time">{{seller.deliveryTime}}分钟</span>
        </div>
      </div>
    </div>
    <split></split>
    <ratingselect @onlycot="method01" @selecttp="method02" :selectType="selectType" :onlyContent="onlyContent" :ratings="ratings"></ratingselect>
    <div class="rating-wrapper">
      <ul>
        <template  v-for="rating in ratings">
          <li class="rating-item" v-show="needShow(rating.rateType,rating.text)">
            <div class="avatar">
              <img :src="rating.avatar" width="28px" height="28px">
            </div>
            <div class="content">
              <h1 class="name">{{rating.username}}</h1>
              <div class="star-wrapper">
                <star :size="24" :score="rating.score"></star>
              </div>
              <span v-show="rating.deliveryTime" class="delivery">{{rating.deliveryTime}}分钟送达</span>
              <span class="text">{{rating.text}}</span>
              <div class="recommend" v-show="rating.recommend && rating.recommend.length">
              <span class="recommend-img">
                <img src="./positive.png" width="30px" height="30px">
              </span>
                <span v-for="item in rating.recommend" class="recommend-item">{{item}}</span>
              </div>
              <div class="time">{{rating.rateTime | filterTime}}</div>
            </div>
          </li>
        </template>
      </ul>
    </div>
  </div>
</div>

</template>

<script>
  import star from '../../components/star/star.vue';
  import split from '../split/split.vue';
  import ratingselect from '../ratingselect/ratingselect.vue';
  import BScroll from 'better-scroll';

  const ALL = 2;
  const POSITIVE = 0;
  const NEGATIVE = 1;
  const ERR_OK = 0;

  export default{
    props:{
      seller:{
        type:Object
      }
    },
    data(){
      return{
        ratings:[],
        showFlag:false,
        selectType:ALL,
        onlyContent:true
      }
    },
    methods:{
      needShow(type,text) {
        if(this.onlyContent && !text){
          return false;
        }
        if(this.selectType === ALL){
          return true;
        }else{
          return type === this.selectType;
        }
      },
      method01(oct) {
        this.onlyContent = oct;
      },
      method02(tp) {
        this.selectType = tp;
      },
    },
    created(){
      this.$http.get('/api/ratings').then((response) => {
        response=response.body;
        if(response.errno===ERR_OK){
          this.ratings=response.data;
          this.$nextTick(() => {
            if(!this.ratscroll){
              this.ratscroll = new BScroll((this.$refs.rat),{click:true});
            }else{
              this.ratscroll.refresh();
            }
          });
        }
      });
    },
    filters:{
      filterTime(time){
        var ratetime = new Date(time);
        return ratetime.getFullYear() + "-" + (ratetime.getMonth() + 1) + "-" + ratetime.getDate() + " " + ratetime.getHours() + ":" + ratetime.getMinutes();
      }
    },
    components:{
      star,
      split,
      ratingselect
    }
  };
</script>
<style>
  .ratings{
    position: absolute;
    top: 174px;
    bottom: 0;
    width: 100%;
    overflow: hidden;
  }
  .ratings .overview{
    display: flex;
    padding: 18px 0;
  }
  .ratings .overview .overview-left{
    flex: 0 0 125px;
    width: 125px;
    border-right: 1px solid rgba(7,17,27,0.1);
    text-align: center;
  }
  .ratings .overview .overview-left .score{
    height: 28px;
    line-height: 28px;
    font-size: 24px;
    color: rgb(255,153,0);
    margin-bottom: 6px;
  }
  .ratings .overview .overview-left .title{
    height: 12px;
    line-height: 12px;
    font-size: 12px;
    color: rgb(7,17,27);
    margin-bottom: 8px;
  }
  .ratings .overview .overview-left .rank{
    height: 10px;
    line-height: 10px;
    font-size: 10px;
    color: rgb(147,153,159);
    margin-bottom: 6px;
  }
  .ratings .overview .overview-right{
    flex: 1;
    padding-left: 10px;
  }
  .ratings .overview .overview-right .score-wrapper{
    height: 24px;
    line-height: 24px;
    margin-bottom: 8px;
  }
  .ratings .overview .overview-right .score-wrapper .title{
    margin-right: 8px;
    font-size: 12px;
    color: rgb(7,17,27);
  }
  .ratings .overview .overview-right .score-wrapper .star{
    display: inline-block;
    vertical-align: top;
    margin-right: 8px;
  }
  .ratings .overview .overview-right .score-wrapper .score{
    font-size: 12px;
    color: rgb(255,153,0);
  }
  .ratings .overview .overview-right .time-wrapper{
    height: 24px;
    line-height: 24px;
    margin-bottom: 8px;
  }
  .ratings .overview .overview-right .time-wrapper .title{
    margin-right: 8px;
    font-size: 12px;
    color: rgb(7,17,27);
  }
  .ratings .overview .overview-right .time-wrapper .time{
    font-size: 12px;
    color: rgb(147,153,159);
  }
  .ratings .rating-wrapper .rating-item{
    display: flex;
    padding: 18px;
    border-bottom: 1px solid rgba(7,17,27,0.1);
  }
  .ratings .rating-wrapper .rating-item .avatar{
    flex: 0 0 28px;
    width: 28px;
    width: 28px;
    height: 28px;
    border-radius: 50%;
    margin-right: 12px;
  }
  .ratings .rating-wrapper .rating-item .avatar img{
    border-radius: 50%;
  }
  .ratings .rating-wrapper .rating-item .content{
    position: relative;
    flex: 1;
  }
  .ratings .rating-wrapper .rating-item .content .name{
    height: 12px;
    line-height: 12px;
    margin-bottom: 4px;
    font-size: 10px;
    color: rgb(7,17,27);
  }
  .ratings .rating-wrapper .rating-item .star-wrapper{
    display: inline-block;
    margin-bottom: 6px;
    margin-right: 6px;
  }
  .ratings .rating-wrapper .rating-item .delivery{
    height: 12px;
    line-height: 12px;
    font-size: 10px;
    font-weight: 200;
    color: rgb(147,153,159);
  }
  .ratings .rating-wrapper .rating-item .text{
    display: block;
    line-height: 18px;
    font-size: 12px;
    color: rgb(7,17,27);
  }
  .ratings .rating-wrapper .rating-item .recommend{
    margin-top: 8px;
  }
  .ratings .rating-wrapper .rating-item .recommend .recommend-img{
    display: inline-block;
    height: 30px;
    line-height: 30px;
  }
  .ratings .rating-wrapper .rating-item .recommend .recommend-item{
    display: inline-block;
    height: 30px;
    line-height: 30px;
    vertical-align: top;
    font-size: 9px;
    color: rgb(147,153,159);
    margin-right:8px ;
    padding: 0px 6px;
    border: 1px solid rgba(7,17,27,0.1);
    border-radius: 1px;
  }
  .ratings .rating-wrapper .rating-item .time{
    position: absolute;
    right: 18px;
    top: 0;
    height: 12px;
    line-height: 12px;
    font-size: 10px;
    font-weight: 200;
    color: rgb(147,153,159);
  }
</style>
