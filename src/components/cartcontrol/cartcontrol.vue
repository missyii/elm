<template>
  <div class="cartcontrol">
    <transition name="move">
      <div class="cart-decrease" v-show="food.count>0" @click.stop.prevent="decrease">
        <img src="./decrease.jpg" width="24px"/>
      </div>
    </transition>
    <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    <div class="cart-add" @click.stop.prevent="add">
      <img src="./add.png" width="24px"/>
    </div>
  </div>

</template>

<script>
  import Vue from 'vue';
  export default{

    props:{
      food:{
        type:Object
      }
    },
    methods:{
      decrease(){
        if(this.food.count){
          this.food.count--;
        }
      },
      add() {
        if(!this.food.count){
          Vue.set(this.food,'count',1);
        }else{
          this.food.count++;
        }
      }
    }
  };
</script>
<style>
  .cartcontrol{
    font-size: 0;
    width: 100%;
    height: 35px;
  }
  .cartcontrol .cart-decrease{
    float: left;
    display: inline-block;
    padding: 6px;
    width: 24px;
    height: 24px;
  }
  .move-enter-active, .move-leave-active {
    transition: all .5s;
  }
  .move-enter, .move-leave-to {
    transform: translateX(20px) rotate(360deg);
    opacity: 0;
  }
  .cartcontrol .cart-count{
    display: inline-block;
    line-height: 35px;
    font-size: 10px;
    color: rgb(147,153,159);
    width: 12px;
    height: 35px;
    margin: 0 6px 0 7px;
  }
  .cartcontrol .cart-add{
    float: right;
    display: inline-block;
    padding: 6px;
    width: 24px;
    height: 24px;
  }
</style>
