<template>
  <div class="header">
    <div class="content-wrapper">
      <div class="avatar">
        <img width="64" height="64" v-bind:src="seller.avatar"/>
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div v-if="seller.supports" class="supports">
          <span class="icon" v-bind:class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div v-if="seller.supports" class="supports_count" v-on:click="showDetail">
        {{seller.supports.length}}个<span style="font-weight: bold; font-size: 14px;margin-left: 2px">></span>
      </div>
    </div>
    <div class="bulletin-wrapper" v-on:click="showDetail">
      <span class="bulletin-title"></span>
      <span class="bulletin-text">{{seller.bulletin}}</span>
      <span class="bulletin-icon">></span>
    </div>
    <div class="background">
      <img v-bind:src="seller.avatar" width="100%" height="100%">
    </div>
    <transition name="fade">
      <div v-show="detailShow" class="detail">
        <div class="detail-wrapper clearfix">
          <div class="detail-main">
            <h1 class="name">{{seller.name}}</h1>
            <div class="star-wrapper">
              <star v-bind:size="48" v-bind:score="seller.score"></star>
            </div>
            <div class="line-title">
              <div class="line"></div>
              <div class="text">优惠信息</div>
              <div class="line"></div>
            </div>
            <ul v-if="seller.supports">
              <li class="support-item" v-for="(item,index) in seller.supports">
                <span class="icon" v-bind:class="classMap[seller.supports[index].type]"></span>
                <span class="text">{{seller.supports[index].description}}</span>
              </li>
            </ul>
            <div class="line-title">
              <div class="line"></div>
              <div class="text">商家公告</div>
              <div class="line"></div>
            </div>
            <div class="bulletin">
              <p class="contents">{{seller.bulletin}}</p>
            </div>
          </div>
        </div>
        <div class="detail-close" v-on:click="hideDetail">×</div>
      </div>
    </transition>
  </div>

</template>
<script>

  import star from '../../components/star/star.vue';

  export default {
    props:{
      seller:{
        type:Object
      }
    },
    data(){
      return {
        detailShow:false
      }
    },
    methods:{
      showDetail() {
        this.detailShow = true;
      },
      hideDetail() {
        this.detailShow = false;
      }
    },
    created(){
      this.classMap=['decrease','discount','special','invoice','guarantee']
    },
    components: {
      star
    }
  }
</script>
<style>
  .header{
    position: relative;
    color: white;
    background-color: rgba(7,17,27,0.5);
  }
  .header .content-wrapper{
    padding: 24px 12px 18px 24px;
    font-size: 0px;
    position: relative;
  }
  .header .content-wrapper .supports_count{
    box-sizing: border-box;
    position: absolute;
    right: 12px;
    bottom: 10px;
    height: 24px;
    width: 50px;
    font-size: 10px;
    font-weight: 200;
    color: white;
    line-height: 12px;
    padding: 7px 8px 7px 8px;
    background-color: rgba(0,0,0,0.2);
    border-radius: 16px;
  }
  .header .content-wrapper .avatar{
    display: inline-block;
    vertical-align: top;
  }
  .header .content-wrapper .avatar img{
    border-radius: 2px;
  }
  .header .content-wrapper .content{
    display: inline-block;
    margin-left: 16px;
  }
  .header .content-wrapper .content .title{
    margin-left: 2px;
    margin-bottom: 8px;
  }
  .header .content-wrapper .content .title .brand{
    display: inline-block;
    vertical-align: top;
    width: 30px;
    height: 18px;
    background: url("brand@2x.png") no-repeat;
    background-size: 30px 18px;
  }
  .header .content-wrapper .content .title .name{
    font-size: 16px;
    font-weight: bold;
    line-height: 18px;
    margin-left: 6px;
  }
  .header .content-wrapper .content .description{
    font-size: 12px;
    font-weight: 200;
    line-height: 12px;
    margin-bottom: 10px;
  }
  .header .content-wrapper .content .supports .icon{
    display: inline-block;
    vertical-align: top;
    width: 18px;
    height: 18px;
    background-size: 18px 18px;
  }
  .icon.decrease{
    display: inline-block;
    width: 18px;
    height: 18px;
    background: url("decrease_1@3x.png") no-repeat;
    background-size: 18px 18px;
  }
  .icon.discount{
    display: inline-block;
    width: 18px;
    height: 18px;
    background: url("discount_1@2x.png") no-repeat;
    background-size: 18px 18px;
  }
  .icon.guarantee{
    display: inline-block;
    width: 18px;
    height: 18px;
    background: url("guarantee_1@2x.png") no-repeat;
    background-size: 18px 18px;
  }
  .icon.invoice{
    display: inline-block;
    width: 18px;
    height: 18px;
    background: url("invoice_1@2x.png") no-repeat;
    background-size: 18px 18px;
  }
  .icon.special{
    display: inline-block;
    width: 18px;
    height: 18px;
    background: url("special_1@2x.png") no-repeat;
    background-size: 18px 18px;
  }
  .header .content-wrapper .content .supports .text{
    margin-left: 4px;
    line-height: 18px;
    font-size: 10px;
    font-weight: 200;
  }
  .header .bulletin-wrapper{
    position: relative;
    height: 28px;
    line-height: 28px;
    padding: 0px 22px 0px 12px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    background-color: rgba(7,17,27,0.2);
  }
  .header .bulletin-wrapper .bulletin-title{
    display: inline-block;
    width: 22px;
    height: 12px;
    background: url("bulletin@2x.png") no-repeat;
    background-size: 22px 12px;
  }
  .header .bulletin-wrapper .bulletin-text{
    font-size: 10px;
    font-weight: 200;
    margin: 0px 4px;
  }
  .header .bulletin-wrapper .bulletin-icon{
    position: absolute;
    right: 12px;
    top: 2px;
    font-size: 14px;
    font-weight: bold;
  }
  .header .background{
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: -1;
    filter: blur(10px);
  }
  .header .detail{
    position: fixed;
    top: 0;
    left: 0;
    z-index: 100;
    width: 100%;
    height: 100%;
    overflow: auto;
    background-color: rgba(7,17,27,0.8);
    transition: all 0.5s;
    -webkit-backdrop-filter: blur(10px);
  }
  .fade-enter-active, .fade-leave-active {
    transition: all .5s;
  }
  .fade-enter, .fade-leave-to {
    opacity: 0;
  }
  .header .detail .clearfix{
    display:inline-block;
  }
  .header .detail .clearfix:after{
    display:block;
    content:" ";
    height: 0;
    line-height: 0;
    clear:both;

  }
  .header .detail .detail-wrapper{
    min-height: 100%;
    width: 100%;
  }
  .header .detail .detail-wrapper .detail-main{
    margin-top: 64px;
    padding-bottom: 64px;
  }
  .header .detail .detail-wrapper .detail-main .name{
    line-height: 32px;
    font-size: 16px;
    font-weight: 700;
    text-align: center;
  }
  .header .detail .detail-wrapper .detail-main .star-wrapper{
    margin-top: 16px;
    margin-bottom: 28px;
    text-align: center;
  }
  .header .detail .detail-wrapper .detail-main .line-title{
    display: flex;
    width:80%;
    margin: 0 auto 24px auto;
  }
  .header .detail .detail-wrapper .detail-main .line-title .line{
    flex:1;
    position: relative;
    top:-6px ;
    border-bottom: 1px solid rgba(255, 255, 255, 0.2);
  }
  .header .detail .detail-wrapper .detail-main .line-title .text{
    padding:0 12px;
    font-size: 14px;
    font-weight: 700;
  }
  .header .detail .detail-wrapper .detail-main ul{
    list-style: none;
    width: 80%;
    margin:28px auto;
  }
  .header .detail .detail-wrapper .detail-main ul .support-item{
    margin-bottom: 12px;
    font-size: 12px;
    font-weight: 200;
    line-height: 18px;
  }
  .header .detail .detail-wrapper .detail-main ul .support-item .icon{
    vertical-align: top;
    margin-right: 6px;
  }
  .header .detail .detail-wrapper .detail-main .bulletin{
    width: 80%;
    margin: 0 auto;
  }
  .header .detail .detail-wrapper .detail-main .bulletin .contents{
    padding: 0 12px;
    line-height: 24px;
    font-size: 12px;
    font-weight: 200;

  }
  .header .detail .detail-close{
    position:relative;
    width: 32px;
    height: 32px;
    margin: -64px auto 0 auto;
    font-size: 50px;
    text-align: center;
    clear:both;
  }
</style>
