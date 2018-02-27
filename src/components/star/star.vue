<template>
  <div class="star" v-bind:class="starType">
    <span v-for="itemClass in itemClasses" v-bind:class="itemClass" class="star-item" track-by="$index"></span>
  </div>
</template>

<script>
  const LENGTH = 5;
  const CLS_ON = 'on';
  const CLS_HALF = 'half';
  const CLS_OFF = 'off';

  export default {
    props:{
      size:{
        type:Number
      },
      score:{
        type:Number
      }
    },
    computed:{
      starType() {
        return 'star-' + this.size
      },
      itemClasses() {
        var result=[];
        var score = Math.floor(this.score*2)/2;
        var hasDecimal = score % 1 !== 0;
        var integer = Math.floor(score);
        for(var i=0; i<integer; i++){
          result.push(CLS_ON)
        }
        if(hasDecimal){
            result.push(CLS_HALF);
          }
        while(result.length<LENGTH){
          result.push(CLS_OFF)
        }
         return result;
      }
    }
  }
</script>
<style>
  .star{
    font-size: 0px;
  }
  .star.star-48 .star-item{
    display: inline-block;
    width: 30px;
    height: 30px;
    margin-right: 22px;
    background-size: 30px 30px;

  }
  .star.star-48 .star-item.on{
    background-image: url("star48_on@3x.png");
  }
  .star.star-48 .star-item.half{
    background-image: url("star48_half@3x.png");
  }
  .star.star-48 .star-item.off{
    background-image: url("star48_off@3x.png");
  }
  .star.star-36 .star-item{
    display: inline-block;
    width: 22px;
    height: 22px;
    margin-right: 8px;
    background-size: 22px 22px;
  }
  .star.star-36 .star-item.on{
    background-image: url("star36_on@3x.png");
  }
  .star.star-36 .star-item.half{
    background-image: url("star36_half@3x.png");
  }
  .star.star-36 .star-item.off{
    background-image: url("star36_off@3x.png");
  }
  .star.star-24 .star-item{
    display: inline-block;
    width: 15px;
    height: 15px;
    margin-right: 3px;
    background-size: 15px 15px;
  }
  .star.star-24 .star-item.on{
    background-image: url("star24_on@3x.png");
  }
  .star.star-24 .star-item.half{
    background-image: url("star24_half@3x.png");
  }
  .star.star-24 .star-item.off{
    background-image: url("star24_off@3x.png");
  }
  .star .star-item{
     background-repeat: no-repeat;
   }
  .star .star-item:last-child{
    margin-right: 0px;
  }


</style>
