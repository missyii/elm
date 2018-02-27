<template>
  <transition name="food-move" >
    <div class="food" v-show="showFlag" ref="fod">
      <div class="food-content">
        <div class="image-header">
          <img :src="food.image"/>
          <span class="back" @click="hide">《 返回</span>
        </div>
        <div class="content">
          <h2 class="name">{{food.name}}</h2>
          <div class="extra">
            <span class="count">月售{{food.sellCount}}份</span>
            <span>好评率{{food.rating}}%</span>
          </div>
          <div class="price">
            <span class="now-price">￥{{food.price}}</span>
            <span v-show="food.oldPrice" class="old-price">￥{{food.oldPrice}}</span>
          </div>
          <div class="cartcontrol-wrapper">
            <cartcontrol :food="food"></cartcontrol>
          </div>
          <div @click.stop.prevent="addFirst" class="add-to-shopcart" v-show="!food.count || food.count === 0">
            加入购物车
          </div>
        </div>
        <split></split>
        <div class="info" v-show="food.info">
          <div class="title">商品介绍</div>
          <div class="text">{{food.info}}</div>
        </div>
        <split v-show="food.info"></split>
        <div class="rating">
          <div class="title">商品评价</div>
          <ratingselect @onlycot="method01" @selecttp="method02" ref="ratingselect" :selectType="selectType" :onlyContent="onlyContent" :desc="desc" :ratings="food.ratings"></ratingselect>
          <div class="rating-wrapper">
            <ul v-show="food.ratings && food.ratings.length">
              <li v-show="needShow(rating.rateType,rating.text)" v-for="rating in food.ratings" class="rating-item">
                <div class="user">
                  <span class="user-name">{{rating.username}}</span>
                  <img :src="rating.avatar" width="12px" height="12px">
                </div>
                <div class="time">{{rating.rateTime | filterTime}}</div>
                <p class="text">
                  <span v-show="rating.rateType === 0">
                    <img src="./positive.png" width="30px" height="30px">
                  </span>
                  <span v-show="rating.rateType === 1">
                    <img src="./negative.png" width="25px" height="25px">
                  </span>
                  <span class="rating-text">{{rating.text}}</span>
                </p>
              </li>
            </ul>
            <div class="no-rating" v-show="!food.ratings || !food.ratings.length">暂无评价</div>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
  import BScroll from 'better-scroll';
  import cartcontrol from '../cartcontrol/cartcontrol.vue';
  import Vue from 'vue';
  import split from '../split/split.vue';
  import ratingselect from '../ratingselect/ratingselect.vue';

  const POSITIVE = 0;
  const NEGATIVE = 1;
  const ALL = 2;

  export default {
    props:{
      food:{
        type:Object
      }
    },
    data(){
      return{
        showFlag:false,
        selectType:ALL,
        onlyContent:true,
        desc:{
          'all':'全部',
          'positive':'推荐',
          'negative':'吐槽'
        }
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
      show() {
        this.showFlag = true;
        this.selectType = ALL;
        this.onlyContent = true;
        this.$nextTick(() => {
          if(!this.scroll){
            this.scroll = new BScroll(this.$refs.fod,{
              click: true
            })
          }else{
            this.scroll.refresh();
          }
        })
      },
      hide() {
        this.showFlag = false;
      },
      addFirst(event){
        Vue.set(this.food,'count',1)
      }
    },
    events:{
      "this.$refs.ratingselect.select"(type){
        this.selectType = type;
      },
      "this.$refs.ratingselect.toggle"(onlyContent){
        this.onlyContent = onlyContent;
      }
    },
    filters:{
      filterTime(time){
        var ratetime = new Date(time);
        return ratetime.getFullYear() + "-" + (ratetime.getMonth() + 1) + "-" + ratetime.getDate() + " " + ratetime.getHours() + ":" + ratetime.getMinutes();
      }
    },
    components:{
      cartcontrol,
      split,
      ratingselect
    }
  }
</script>
<style>
  .food{
    position: fixed;
    top: 0;
    left: 0;
    bottom: 48px;
    width: 100%;
    z-index: 30;
    background: white;
    overflow: hidden;
  }
  .food-move-enter-active, .food-move-leave-active {
    transition: all .3s;
  }
  .food-move-enter, .food-move-leave-to {
    transform: translateX(100%);
  }
  .food .food-content .image-header{
    position: relative;
    width: 100%;
    height: 0;
    padding-top: 100%;
  }
  .food .food-content .image-header img{
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
  }
  .food .food-content .image-header .back{
    display: inline-block;
    position: absolute;
    top: 10px;
    left: 0px;
    padding: 10px;
    font-size: 16px;
    color: blue;
    font-weight: 700;
  }
  .food .food-content .content{
    position: relative;
    padding: 18px;
    width: 100%;
  }
  .food .food-content .title{
    margin-bottom: 6px;
    height: 24px;
    line-height: 24px;
    font-size: 14px;
    font-weight: 500;
    color: rgb(17,17,27);
  }
  .food .food-content .content .name{
    height: 10px;
    line-height: 10px;
    font-size: 14px;
    font-weight: 700;
    color: rgb(17,17,27);
  }
  .food .food-content .content .extra{
    margin: 8px 0 18px 0;
    height: 10px;
    line-height: 10px;
    font-size: 10px;
    color: rgb(147,153,159);
  }
  .food .food-content .content .extra span{
    margin-right: 12px;
  }
  .food .food-content .content .price{
    line-height: 24px;
  }
  .food .food-content .content .price .now-price{
    margin-right: 12px;
    font-weight: 700;
    font-size: 14px;
    color: rgb(240,20,20);
  }
  .food .food-content .content .price .old-price{
    text-decoration: line-through;
    font-size: 10px;
    font-weight: 700;
    color: rgb(147,153,159);
  }
  .food .food-content .content .cartcontrol-wrapper{
    position: absolute;
    left: 57%;
    bottom:18px;
    width: 100px;
    height: 35px;
  }
  .food .food-content .content .add-to-shopcart{
    position: absolute;
    left: 57%;
    bottom:18px;
    width: 100px;
    height: 35px;
    line-height: 35px;
    text-align: center;
    box-sizing: border-box;
    padding:1px 18px 1px 18px;
    z-index: 10;
    color: white;
    border-radius: 24px;
    background: rgb(0,160,220);
    font-size: 10px;

  }
  .food .food-content .info{
    padding: 18px;
  }
  .food .food-content .info .text{
    margin:0 8px;
    font-size: 12px;
    font-weight: 200;
    line-height: 24px;
    color: rgb(77,85,93);
  }
  .food .food-content .rating{
    padding-top: 18px;
  }
  .food .food-content .rating .title{
    margin: 0 0 0 18px;
  }
  .food .food-content .rating .rating-wrapper{
    padding:0 18px;
  }
  .food .food-content .rating .rating-wrapper .rating-item{
    position: relative;
    padding: 16px 0;
    list-style: none;
    border-bottom: 1px solid rgba(7,17,27,0.1);
  }
  .food .food-content .rating .rating-wrapper .rating-item .text .rating-text{
    vertical-align: top;
    display: inline-block;
    height: 30px;
    line-height: 30px;
    margin-left: 4px;
  }
  .food .food-content .rating .rating-wrapper .rating-item .user{
    position: absolute;
    right: 0;
    top: 16px;
    line-height: 12px;
    height: 12px;
  }
  .food .food-content .rating .rating-wrapper .rating-item .user .user-name{
    font-size: 10px;
    color: rgb(147,153,159);
    margin-right: 6px;
  }
  .food .food-content .rating .rating-wrapper .rating-item .user img{
    border-radius: 50%;
  }
  .food .food-content .rating .rating-wrapper .rating-item .time{
    height: 12px;
    line-height: 12px;
    font-size: 10px;
    color: rgb(147,153,159);
    margin-bottom: 6px;
  }
</style>
