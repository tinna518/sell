<template>
  <div id="app">
    <top :seller="seller"></top>
    <div class="tabs">
      <div class="tab-item"><router-link to="/goods">商品</router-link></div>
      <div class="tab-item"><router-link to="/ratings">评价</router-link></div>
      <div class="tab-item"> <router-link to="/seller">商家</router-link></div>
    </div>
    <router-view :seller="seller"></router-view>
  </div>
</template>

<script>
import top from './components/top/top.vue'
// const ERR_OK = 0

export default {
  data () {
    return {
      seller: {}
    }
  },
  created () {
    this.$axios('/api/seller').then((response) => {
      if (response.data.erron === 0) {
        this.seller = response.data.data
      }
    })
  },
  name: 'App',
  components: {
    top: top
  }
}
</script>

<style lang="less" scoped>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  .tabs{
     display: flex;
     width: 100%;
     height: 40px;
     line-height: 40px;
     border-bottom: 1px solid rgba(7,17,27,0.1);
    .tab-item{
         flex:1;
         text-align: center;
           & > a {
           font-size: 14px;
           color: reb(77,85,93);
             &.router-link-exact-actiev{
            color: red;
         }
      }
     }
 }

}
</style>
