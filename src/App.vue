<template>
  <div id="app">
    <my-header v-bind:seller="seller"></my-header>
    <div class="tab">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <router-view :seller="seller"></router-view>
  </div>
</template>

<script>
  import myHeader from './components/header/myHeader.vue';

  const ERR_OK = 0;

  export default {
    data() {
      return {
        seller:{}
      };
    },
    created() {
      this.$http.get('/api/seller').then((response) => {
        response=response.body;
        if(response.errno===ERR_OK){
          this.seller=response.data;
        }
      });
    },
    components:{
      myHeader: myHeader
    }
  }
</script>

<style>
  #app .tab{
    display: flex;
    width: 100%;
    height: 40px;
    line-height: 40px;
    border-bottom: 1px solid rgba(7,17,27,0.2);
  }
  #app .tab .tab-item{
    flex: 1;
    text-align: center;
  }
  #app .tab .tab-item a{
    display:block;
    text-decoration: none;
    font-size: 14px;
    color: rgb(77,85,93);
  }
  #app .tab .tab-item .router-link-active{
    color: rgb(240,20,20);
  }
</style>
