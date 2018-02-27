<template>
  <div class="shopcart">
      <div class="content" @click="toggleList">
        <div class="content-left">
          <div class="logo-wrapper">
            <img class="logo" src="./shortcart.png" :class="{'highlight':totalCount>0}">
            <div class="num" v-show="totalCount>0">{{totalCount}}</div>
          </div>
          <div class="price" :class="{'highlight':totalPrice>0}">￥{{totalPrice}}元</div>
          <div class="desc">另需配送费￥{{deliveryPrice}}元</div>
        </div>
        <div class="content-right">
          <span class="pay" :class="{'highlight':totalPrice>=minPrice}" @click.stop.prevent="pay">{{payDesc}}</span>
        </div>
      </div>
      <transition name="fold">
        <div class="shopcart-list" v-show="listShow">
          <div class="list-background" @click="listHide"></div>
          <div class="list-header">
            <h1 class="title">购物车</h1>
            <span class="empty" @click="empty">清空</span>
          </div>
          <div class="list-content" ref="list">
            <ul>
              <li class="food" v-for="food in selectFoods">
                <span class="name-wrapper">{{food.name}}</span>
                <span class="price-wrapper">￥{{food.price * food.count}}</span>
                <div class="shopcart-wrapper">
                  <cartcontrol :food="food"></cartcontrol>
                </div>
              </li>
            </ul>
          </div>

        </div>
      </transition>
    </div>
</template>

<script>
  import cartcontrol from '../cartcontrol/cartcontrol.vue';
  import BScroll from 'better-scroll';

  export default {
    props:{
      selectFoods:{
        type:Array,
        default(){
          return [];
        }
      },
      deliveryPrice:{
        type:Number,
        default:0
      },
      minPrice:{
        type:Number,
        default:0
      }
    },
    data(){
      return{
        fold:true
      }
    },
    methods:{
      toggleList() {
        if(!this.totalCount){
          return;
        }else{
          this.fold = !this.fold;
        }
      },
      empty() {
        this.selectFoods.forEach((food) => {
          food.count = 0;
        })
      },
      listHide(){
        this.fold = true;
      },
      pay() {
        if(this.totalPrice < this.minPrice){
          return;
        }
        window.alert(`请支付${this.totalPrice}元`);
      }
    },
    computed:{
      totalPrice(){
        var total=0;
        this.selectFoods.forEach((food) => {
          total += food.price * food.count;
        });
        return total;
      },
      totalCount(){
        var count = 0;
        this.selectFoods.forEach((food) => {
          count += food.count;
        });
        return count;
      },
      payDesc(){
        if(this.totalPrice === 0){
          return `￥${this.minPrice}元起送`;
        }else if(this.totalPrice < this.minPrice){
          let diff=this.minPrice - this.totalPrice;
          return `还差￥${diff}元起送`;
        }else{
          return '结算';
        }
      },
      listShow() {
        if(!this.totalCount){
          this.fold = true;
          return false;
        }
        let show = !this.fold;
        if(show){
          this.$nextTick(() => {
          if(!this.scroll){
            this.scroll = new BScroll(this.$refs.list,{
              click:true
            })
          }else{
            this.scroll.refresh();
          }

        });
        }
        return show;
      }
    },
    components:{
      cartcontrol
    },
  }
</script>
<style>
.shopcart{
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 48px;
  z-index: 50;
}
.shopcart .content{
  display: flex;
  background: #141d27;
  font-size: 0;
}
.shopcart .content .content-left{
  flex: 1;
}
.shopcart .content .content-left .logo-wrapper{
  display: inline-block;
  position: relative;
  top: -10px;
  margin: 0 12px;
  padding: 6px;
  width: 56px;
  height: 56px;
  box-sizing: border-box;
  vertical-align: top;
  border-radius: 50%;
  background:#141d27;
}
.shopcart .content .content-left .logo-wrapper .logo{
  width: 100%;
  height: 100%;
  border-radius: 50%;
  background: #2b343c;
}
.shopcart .content .content-left .logo-wrapper .logo.highlight{
  background: rgb(0,160,220);
}
.shopcart .content .content-left .logo-wrapper .num{
  position: absolute;
  top: 0;
  right: 0;
  width: 24px;
  height: 16px;
  line-height: 16px;
  text-align: center;
  border-radius: 16px;
  font-size: 9px;
  font-weight: 700;
  color: #fff;
  background:rgb(240,20,20);
  box-shadow: 0 4px 8px 0 rgba(0,0,0,0.4);
}
.shopcart .content .content-left .price{
  display: inline-block;
  vertical-align: top;
  margin-top: 12px;
  line-height: 24px;
  box-sizing: border-box;
  padding-right: 12px;
  border-right: 1px solid rgba(255,255,255,0.1);
  font-size: 16px;
  font-weight: 700;
  color: rgba(255,255,255,0.4);
}
.shopcart .content .content-left .price.highlight{
  color: #fff;
}
.shopcart .content .content-left .desc{
  display: inline-block;
  vertical-align: top;
  line-height: 24px;
  margin-top: 12px;
  margin-left: 12px;
  font-size: 12px;
  font-weight: 300;
  color: rgba(255,255,255,0.4);
}
.shopcart .content .content-right{
  flex: 0 0 120px;
  width: 105px;
  height: 100%;
  background-color: #2b333b;
}
.shopcart .content .content-right .pay{
  display: block;
  margin: 0 auto;
  height: 48px;
  width: 100%;
  line-height: 48px;
  font-size: 12px;
  text-align: center;
  font-weight: 700;
  color: rgba(255,255,255,0.4);
}
.shopcart .content .content-right .pay.highlight{
  background: #00b43c;
  font-size: 16px;
  color: white;
}
.shopcart .shopcart-list{
  position: absolute;
  bottom: 100%;
  left: 0;
  z-index:-1 ;
  width: 100%;
}
.fold-enter-active, .fold-leave-active {
  transition: all .5s;
}
.fold-enter, .fold-leave-to {
  transform: translateY(100%);
}
.shopcart .shopcart-list .list-header{
  height: 40px;
  line-height: 40px;
  padding: 0 18px;
  background: #f3f5f7;
  border-bottom: 1px solid rgba(7,17,27,0.1);
}
.shopcart .shopcart-list .list-header .title{
  float: left;
  font-size: 14px;
  font-weight: 400;
  color: rgb(7,17,27);
}
.shopcart .shopcart-list .list-header .empty{
  float: right;
  font-size: 12px;
  color: rgb(0,160,220);
}
.shopcart .shopcart-list .list-content{
  padding: 0 18px;
  max-height: 217px;
  background: #fff;
  overflow: hidden;
}
.shopcart .shopcart-list .list-content .food{
  list-style: none;
  margin: 0 auto;
  position: relative;
  padding: 12px 0;
  box-sizing: border-box;
  width: 90%;
  border-bottom: 1px solid rgba(27,27,27,0.1);
}
.shopcart .shopcart-list .list-content .food .name-wrapper{
  display: inline-block;
  height: 24px;
  line-height: 24px;
  font-size: 14px;
  color: rgb(7,17,27);
  font-weight: 700;
}
.shopcart .shopcart-list .list-content .food .price-wrapper{
  position: absolute;
  top: 14px;
  right:112px ;
  height: 24px;
  line-height: 24px;
  font-size: 14px;
  font-weight: 700;
  color: rgb(240,20,20);
}
.shopcart .shopcart-list .list-content .food .shopcart-wrapper{
  position: absolute;
  top: 8px;
  right: 0;
  bottom: 12px;
  width: 100px;
  height: 35px;
}
.list-background{
  z-index: 40;
  width: 100%;
  height: 620px;
  background-color: rgba(7,17,27,0.6);
  -webkit-backdrop-filter:blur(10px);
}
</style>
