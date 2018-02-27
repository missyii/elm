<template>
  <div class="ratingselect">
    <div class="rating-type">
      <span @click="select(2)" class="block positive" :class="{'active':tp === 2}">
        {{desc.all}}
        <span class="count">{{ratings.length}}</span>
      </span>
      <span @click="select(0)" class="block positive" :class="{'active':tp === 0}">
        {{desc.positive}}
        <span class="count">{{positives.length}}</span>
      </span>
      <span @click="select(1)" class="block negative" :class="{'active':tp === 1}">
        {{desc.negative}}
        <span class="count">{{negatives.length}}</span>
      </span>
    </div>
    <div class="switch">
      <span class="select" :class="{active:oct}">
        <span @click="toggle" >√</span>
      </span>
      <span class="text">只看有内容的评价</span>
    </div>
  </div>
</template>

<script>
  const POSITIVE = 0;
  const NEGATIVE = 1;
  const ALL = 2;

  export default {
    props:{
      ratings:{
        type:Array,
        default(){
          return [];
        }
      },
      selectType:{
        type:Number,
        default:ALL
      },
      onlyContent:{
        type:Boolean,
        default:false
      },
      desc:{
        type:Object,
        default(){
          return {
            'all':'全部',
            'positive':'满意',
            'negative':'不满意'
          }
        }
      }
    },
    data(){
      return {
        tp: this.selectType,
        oct:true
      }
    },
    computed:{
      positives() {
        return this.ratings.filter((rating) => {
          return rating.rateType === POSITIVE;
        })
      },
      negatives() {
        return this.ratings.filter((rating) => {
          return rating.rateType === NEGATIVE;
        })
      }
    },
    methods:{
      select(type) {
        this.tp = type;
        this.$emit('selecttp',this.tp);
      },
      toggle() {
        this.oct = !this.oct;
        this.$emit('onlycot', this.oct);
      }
    },
  }
</script>
<style>
  .ratingselect{
    margin-top: 18px;
  }
  .ratingselect .switch{
    padding: 12px 18px;
    border-bottom: 1px solid rgba(7,17,27,0.2);
  }
  .ratingselect .switch .text{
    font-size: 12px;
    color:rgb(147,153,159); ;
  }
  .ratingselect .switch .select{
    display: inline-block;
    width: 20px;
    height: 20px;
    line-height: 20px;
    text-align: center;
    font-size: 18px;
    font-weight: 900;
    color: white;
    background: rgb(147,153,159);
    border-radius: 50%;
    margin-left: 4px;
  }
  .ratingselect .switch .select.active{
    background: #00c850;
  }
  .ratingselect .rating-type{
    padding: 0 0 18px 0;
    margin: 0 18px 0 18px;
    border-bottom: 1px solid rgba(7,17,27,0.1);
  }
  .ratingselect .rating-type .block{
    display: inline-block;
    padding: 16px;
    margin-right: 8px;
    line-height: 16px;
    text-align: center;
    font-size: 14px;
    color: rgb(77,85,93);
  }
  .ratingselect .rating-type .block.active{
    background: rgb(77,85,93);
    color: white;
  }
  .ratingselect .rating-type .block .count{
    font-size: 8px;
  }
  .ratingselect .rating-type .block.positive{
    background: rgba(0,160,220,0.2);
  }
  .ratingselect .rating-type .positive.active{
    background: rgb(0,160,220);
    color: white;
  }
  .ratingselect .rating-type .negative{
    background: rgba(77,85,93,0.2);
  }

</style>
