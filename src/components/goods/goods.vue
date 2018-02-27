<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menu">
      <ul>
        <li v-for="(item,index) in goods" class="menu-item" :class="{'current':currentIndex===index}" @click="selectMenu(index)">
          <span class="text">
            <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>
            {{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" ref="foods">
      <ul>
        <li v-for="item in goods" class="food-list food-list-hook">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li v-for="food in item.foods" class="food-item" @click="selectFood(food,$event)">
              <div class="icon">
                <img :src="food.icon" width="57px"/>
              </div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p class="description">{{food.description}}</p>
                <div class="extra">
                  <span class="count">月售{{food.sellCount}}份</span>
                  <span>好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span class="now-price">￥{{food.price}}</span>
                  <span v-show="food.oldPrice" class="old-price">￥{{food.oldPrice}}</span>
                </div>
                <div class="cart-control-wrapper">
                  <cartcontrol :food="food"></cartcontrol>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <shopcart :select-foods="selectFoods" :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></shopcart>
    <food :food="selectedFood" ref="food"></food>
  </div>
</template>

<script>
  import BScroll from 'better-scroll';
  import shopcart from '../shopcart/shopcart.vue';
  import cartcontrol from '../cartcontrol/cartcontrol.vue';
  import food from '../food/food.vue'

  const ERR_OK = 0;


  export default{
    props:{
      seller:{
        type:Object
      }
    },
    data() {
      return{
        goods:[],
        listHeight:[],
        scrollY:0,
        selectedFood:{}
      };
    },
    computed:{
      currentIndex() {
        for(var i=0; i<this.listHeight.length;i++){
          var height1 = this.listHeight[i];
          var height2 = this.listHeight[i+1];
          if(!height2 || (this.scrollY >= height1 && this.scrollY < height2)){
            return i;
          }
        }
        return 0;
      },
      selectFoods(){
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
    created() {
      this.classMap=['decrease','discount','special','invoice','guarantee'];
      this.$http.get('/api/goods').then((response) => {
        response=response.body;
        if(response.errno===ERR_OK){
          this.goods=response.data;
          this.$nextTick(() => {
            this._initScroll();
            this._calculateHeight();
          })
        }
      });
    },
    methods: {
      selectMenu(index) {
        var foodList = this.$refs.foods.getElementsByClassName('food-list-hook');
        var el = foodList[index];
        this.foodsScroll.scrollToElement(el, 100);
      },
      _initScroll() {
        this.menuScroll = new BScroll(this.$refs.menu, {click: true});
        this.foodsScroll = new BScroll(this.$refs.foods, {
          probeType: 3,
          click: true
        });
        this.foodsScroll.on('scroll', (pos) => {
          this.scrollY = Math.abs(Math.round(pos.y))
        })
      },
      _calculateHeight() {
        var foodList = this.$refs.foods.getElementsByClassName('food-list-hook');
        var height = 0;
        this.listHeight.push(height);
        for (var i = 0; i < foodList.length; i++) {
          var item = foodList[i];
          height += item.clientHeight;
          this.listHeight.push(height);
        }
      },
      selectFood(food,event) {
        if(!event._constructed){
          return;
        }
        this.selectedFood = food;
        var self = this;
        this.selectedFood.ratings.forEach((a) => {
          self.$set(a, 'show', true)
        });
        this.$refs.food.show();
  }
    },
    components:{
      shopcart,
      cartcontrol,
      food
    }
  };
</script>
<style>
  .goods{
    display: flex;
    position: absolute;
    top: 174px;
    bottom: 46px;
    width: 100%;
    overflow: hidden;
  }
  .goods .menu-wrapper{
    flex: 0 0 80px;
    width: 80px;
    background: #f3f5f7;
  }
  .goods .menu-wrapper ul .menu-item{
    list-style: none;
    display: table;
    height: 54px;
    width: 56px;
    line-height: 14px;
    padding:0 12px;
  }
  .goods .menu-wrapper ul .menu-item.current{
    position: relative;
    z-index: 10;
    margin-top:-1px ;
    background: #fff;
    color: rgb(7,17,27);
    font-weight:700 ;
  }
  .goods .menu-wrapper ul .menu-item .icon{
    display: inline-block;
    vertical-align: top;
    width: 18px;
    height: 18px;
    background-size: 18px 18px;
  }
  .menu-item .icon.decrease{
    display: inline-block;
    width: 18px;
    height: 18px;
    background: url("decrease_3@2x.png") no-repeat;
    background-size: 18px 18px;
  }
  .menu-item .icon.discount{
    display: inline-block;
    width: 18px;
    height: 18px;
    background: url("discount_3@2x.png") no-repeat;
    background-size: 18px 18px;
  }
  .menu-item .icon.guarantee{
    display: inline-block;
    width: 18px;
    height: 18px;
    background: url("guarantee_3@2x.png") no-repeat;
    background-size: 18px 18px;
  }
  .menu-item .icon.invoice{
    display: inline-block;
    width: 18px;
    height: 18px;
    background: url("invoice_3@2x.png") no-repeat;
    background-size: 18px 18px;
  }
  .menu-item .icon.special{
    display: inline-block;
    width: 18px;
    height: 18px;
    background: url("special_3@2x.png") no-repeat;
    background-size: 18px 18px;
  }
  .goods .menu-wrapper ul .menu-item{
    font-weight: 200;
    color: rgb(24,20,20);
  }
  .goods .menu-wrapper ul .menu-item .text{
    font-size: 12px;
    display: table-cell;
    width: 56px;
    border-bottom:1px solid rgba(7,17,27,0.1);
    position: relative;
    vertical-align: middle;
  }
  .goods .foods-wrapper{
    flex: 1;
  }
  .goods .foods-wrapper .food-list{
    list-style: none;
  }
  .goods .foods-wrapper .food-list .title{
    height: 26px;
    line-height: 26px;
    padding-left: 14px;
    border-left: 2px solid #d9dde1;
    font-size: 12px;
    color: rgb(147,153,159);
    background-color: #f3f5f7;
  }
  .goods .foods-wrapper .food-list .food-item{
    display: flex;
    padding: 18px;
    border-bottom:1px solid rgba(7,17,27,0.1);
  }
  .goods .foods-wrapper .food-list .food-item:last-child{
    border-bottom: 0;
  }
  .goods .foods-wrapper .food-list .food-item .icon{
    flex: 0 0 57px;
    margin-right: 10px;
  }
  .goods .foods-wrapper .food-list .food-item .content{
    flex: 1;
    position: relative;
  }
  .goods .foods-wrapper .food-list .food-item .content .name{
    margin-bottom: 8px;
    margin-top: 2px;
    height: 14px;
    line-height: 14px;
    font-size: 14px;
    color: rgb(7,17,27);
  }
  .goods .foods-wrapper .food-list .food-item .content .description{
    margin-bottom: 8px;
    font-size: 10px;
    line-height: 15px;
    color: rgb(147,153,159);
  }
  .goods .foods-wrapper .food-list .food-item .content .extra{
    font-size: 10px;
    height: 10px;
    line-height: 10px;
    color: rgb(147,153,159);
    margin-bottom: 8px;
  }
  .goods .foods-wrapper .food-list .food-item .content .extra .count{
    margin-right: 12px;
  }
  .goods .foods-wrapper .food-list .food-item .content .price{
    line-height: 24px;
    margin-left: 18px;
  }
  .goods .foods-wrapper .food-list .food-item .content .price .now-price{
    margin-right: 8px;
    font-weight: 700;
    font-size: 14px;
    color: rgb(240,20,20);

  }
  .goods .foods-wrapper .food-list .food-item .content .price .old-price{
    text-decoration: line-through;
    font-size: 10px;
    font-weight: 700;
    color: rgb(147,153,159);
  }
  .goods .foods-wrapper .food-list .food-item .content .cart-control-wrapper{
    position: absolute;
    right: 0;
    bottom: 0px;
    width: 100px;
    height: 35px;

  }
</style>
