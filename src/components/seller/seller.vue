<template>
<div class="seller"  ref="sel">
  <div class="seller-content">
    <div class="overview">
      <div class="title">{{seller.name}}</div>
      <span class="star-wrapper">
        <star :size="36" :score="seller.score"></star>
      </span>
      <span class="rating-count">（{{seller.ratingCount}}）</span>
      <span class="sell-count">月售{{seller.sellCount}}单</span>
      <div class="collection-wrapper" @click="toggle">
        <img v-show="show" src="./no-collection.jpg" width="26px" height="26px"/>
        <img v-show="!show" src="./collection.jpg" width="26px" height="26px"/>
        <span class="collection">{{collect}}</span>
      </div>
      <ul>
        <li class="block">
          <h2>起送价</h2>
          <div class="content">
            <span class="stress">{{seller.minPrice}}</span>
            <span class="unit">元</span>
          </div>
        </li>
        <li class="block">
          <h2>商家配送</h2>
          <div class="content">
            <span class="stress">{{seller.deliveryPrice}}</span>
            <span class="unit">元</span>
          </div>
        </li>
        <li class="block">
          <h2>平均配送时间</h2>
          <div class="content">
            <span class="stress">{{seller.deliveryTime}}</span>
            <span class="unit">分钟</span>
          </div>
        </li>
      </ul>
    </div>
    <split></split>
    <div class="bulletin">
      <div class="title">公告与活动</div>
      <p class="text">{{seller.bulletin}}</p>
      <ul class="supports" v-if="seller.supports">
        <li v-for="(support,index) in seller.supports" class="support-item">
          <span class="icon" v-bind:class="classMap[seller.supports[index].type]"></span>
          <span class="text">{{seller.supports[index].description}}</span>
        </li>
      </ul>
    </div>
    <split></split>
    <div class="pics">
      <div class="title">商家实景</div>
      <div class="pic-wrapper" ref="wrapper">
        <ul ref="pic">
          <li class="picture-item" v-for="picture in seller.pics">
            <img :src="picture" width="120px" height="90px">
          </li>
        </ul>

      </div>
    </div>
    <split></split>
    <div class="infos">
      <div class="title">商家信息</div>
      <p v-for="info in seller.infos" class="info-item">{{info}}</p>
    </div>
  </div>
</div>

</template>

<script>
  import star from '../../components/star/star.vue';
  import split from '../split/split.vue';
  import BScroll from 'better-scroll';

  export default{
    props:{
      seller:{
        type:Object
      }
    },
    data(){
      return {
        show:false
      }
    },
    computed:{
      collect:function(){
        return this.show? '收藏':'已收藏';
      },
      selectFoods(){
        console.log(this.goods);
        let foods = [];
        this.goods.forEach((good) => {
          good.foods.forEach((food) => {
            if(food.count){
              foods.push(food);
            }
          });
        });
        return foods;
      }
    },
    created(){
      this.classMap=['decrease','discount','special','invoice','guarantee'];
//      this.$http.get('../api/goods').then((response) => {
//          response=response.body;
//          if(response.errno===ERR_OK){
//            this.goods=response.data;
//          }
//        });
    },
    mounted(){
      this._initScroll();
      this._initPics();
    },
    watch:{
      'seller'() {
        this._initScroll();
        this._initPics();
      }
    },
    methods:{
      toggle(event){
        if(!event._constructed){
          return;
        }
        this.show = !this.show;
      },
      _initScroll(){
        if(!this.scroll){
          this.scroll = new BScroll((this.$refs.sel),{click:true});
        }else{
          this.scroll.refresh();
        }
      },
      _initPics(){
        if(this.seller.pics){
          let picWidth = 120;
          let margin = 6;
          let ulWidth = (picWidth + margin) * this.seller.pics.length-margin;
          this.$refs.pic.width = ulWidth + 'px';
          this.$nextTick(() =>{
            if(!this.picScroll) {
              this.picScroll = new BScroll((this.$refs.wrapper), {
                scrollX: true
              })
            }else{
              this.picScroll.refresh();
            }
          })
        }
      }
    },
    components:{
      star,
      split
    }
  };
</script>
<style>
  .seller{
    position: absolute;
    top: 174px;
    bottom: 0;
    width: 100%;
    overflow: hidden;
  }
  .seller .seller-content .overview{
    position: relative;
    padding: 18px;
    font-size: 0;
  }
  .seller .seller-content .overview .title{
    margin-bottom: 8px;
    line-height: 14px;
    height: 14px;
    font-size: 14px;
    color: rgb(7,17,27);
  }
  .seller .seller-content .overview .star-wrapper{
    display: inline-block;
    margin-bottom: 18px;
    margin-right: 8px;
    vertical-align: top;
  }
  .seller .seller-content .overview .rating-count{
    height: 18px;
    line-height: 18px;
    margin-right: 12px;
    font-size: 10px;
    color: rgb(77,85,93);
  }
  .seller .seller-content .overview .sell-count{
    height: 18px;
    line-height: 18px;
    font-size: 10px;
    color: rgb(77,85,93);
  }
  .seller .seller-content .overview .collection-wrapper{
    position: absolute;
    right: 18px;
    top: 18px;
    text-align: center;
  }
  .seller .seller-content .overview .collection-wrapper span{
    display: block;
    font-size: 10px;
    color: rgb(77,85,93);
    height: 10px;
    margin-top: 4px;
    line-height: 10px;
  }
  .seller .seller-content .overview ul{
    display: flex;
    border-top: 1px solid rgba(7,17,27,0.1);
    padding: 18px;
    list-style: none;
  }
  .seller .seller-content .overview ul .block{
    flex: 1;
    border-right: 1px solid rgba(7,17,27,0.1);
  }
  .seller .seller-content .overview ul .block:last-child{
    border-right: 0;
  }
  .seller .seller-content .overview ul .block h2{
    line-height: 10px;
    height: 10px;
    font-size: 10px;
    color: rgb(147,153,159);
    margin-bottom: 4px;
    text-align: center;
  }
  .seller .seller-content .overview ul .block .content{
    height: 24px;
    line-height: 24px;
    text-align: center;
  }
  .seller .seller-content .overview ul .block .content .stress{
    font-size: 24px;
    font-weight: 200;
    color: rgb(7,17,27);
  }
  .seller .seller-content .overview ul .block .content .unit{
    font-size: 10px;
    font-weight: 200;
    color: rgb(7,17,27);
  }
  .seller .seller-content .bulletin{
    padding:18px
  }
  .seller .seller-content .bulletin .title{
    padding-bottom: 8px;
    height: 14px;
    line-height: 14px;
    font-size: 14px;
    color: rgb(7,17,27);
  }
  .seller .seller-content .bulletin .text{
    padding-lft: 12px;
    padding-right: 12px;
    padding-bottom: 16px;
    line-height: 24px;
    font-size: 14px;
    font-weight: 200;
    color:rgb(240,20,20);
    border-bottom: 1px solid rgba(7,17,27,0.1);
  }
  .seller .seller-content .bulletin .support-item{
    list-style: none;
    padding:16px 12px;
    border-bottom: 1px solid rgba(7,17,27,0.1);
  }
  .seller .seller-content .bulletin .support-item:last-child{
    padding-bottom: 0;
    border-bottom: 0;
  }
  .seller .seller-content .bulletin .support-item .icon{

  }
  .seller .seller-content .bulletin .support-item .text{
    height: 16px;
    line-height: 16px;
    font-size: 12px;
    font-weight: 200;
    color: rgb(7,17,27);
    border: 0;
    vertical-align: top;
    box-sizing: border-box;
    pading-top:2px;
  }
  .seller .seller-content .pics{
    padding: 18px;
  }
  .seller .seller-content .pics .title{
    padding-bottom: 8px;
    height: 14px;
    line-height: 14px;
    font-size: 14px;
    color: rgb(7,17,27);
  }
  .seller .seller-content .pics .pic-wrapper{
    width: 100%;
    overflow: hidden;
    white-space: nowrap;

  }
  .seller .seller-content .pics .pic-wrapper .picture-item{
    list-style: none;
    font-size: 0;
    display: inline-block;
    margin-right: 6px;
    width: 120px;
    height: 90px;
  }
  .seller .seller-content .pics .pic-wrapper .picture-item:last-child{
    margin: 0;
  }
  .seller .seller-content .infos{
    padding: 18px;
  }
  .seller .seller-content .infos .title{
    padding-bottom: 8px;
    height: 14px;
    line-height: 14px;
    font-size: 14px;
    color: rgb(7,17,27);
    border-bottom: 1px solid rgba(7,17,27,0.1);
  }
  .seller .seller-content .infos .info-item{
    padding: 16px 12px;
    border-bottom: 1px solid rgba(7,17,27,0.1);
    font-size: 12px;
    font-weight: 200;
    line-height: 16px;
    color: rgb(7,17,27);
  }
  .seller .seller-content .infos .info-item:last-child{
    padding-bottom: 0;
    border-bottom: 0;
  }
  .icon.decrease{
    display: inline-block;
    width: 18px;
    height: 18px;
    background: url("decrease_4@3x.png") no-repeat;
    background-size: 18px 18px;
  }
  .icon.discount{
    display: inline-block;
    width: 18px;
    height: 18px;
    background: url("discount_4@3x.png") no-repeat;
    background-size: 18px 18px;
  }
  .icon.guarantee{
    display: inline-block;
    width: 18px;
    height: 18px;
    background: url("guarantee_4@3x.png") no-repeat;
    background-size: 18px 18px;
  }
  .icon.invoice{
    display: inline-block;
    width: 18px;
    height: 18px;
    background: url("invoice_4@3x.png") no-repeat;
    background-size: 18px 18px;
  }
  .icon.special{
    display: inline-block;
    width: 18px;
    height: 18px;
    background: url("special_4@3x.png") no-repeat;
    background-size: 18px 18px;
  }

</style>
