<template>
    <div class="cartcontrol">
        <div class="cart-decrease "
        @click="decreaseCart" v-show="food.count >0" transition='move'>
        <span class="inner icon-remove_circle_outline"></span>
        </div>
        <div class="cart-count" v-show="food.count >0">{{food.count}}</div>
        <div class="cart-add icon-add_circle" @click="addCart"></div>
    </div>
</template>

<script>
import Vue from 'vue'
export default {
  props: {
    food: {
      type: Object
    }
  },
  created () {
  },
  methods: {
    addCart (event) {
      if (!event._constructed) { return }
      if (!this.food.count) {
        Vue.set(this.food, 'count', 1)
      } else {
        this.food.count++
      }
    },
    decreaseCart () {
      if (!event._constructed) { return }
      if (this.food.count) {
        this.food.count--
      }
    }
  }

}
</script>
<style lang="less" scoped>
@import "../../common/less/_icon.less";
.cartcontrol{
    font-size: 0;
    .cart-decrease{
        display: inline-block;
        padding: 6px;
        transition: all 0.4s linear;
         .inner{
              display: inline-block;
              font-size: 24px;
              line-height: 24px;
              color: rgb(0,160,220);
              transition: all 0.4s linear;
              transform: rotate(0);
              &.move-transition{
               opacity: 1;
               transform: translate3d(0,0,0);}
              &.move-enter,  &.move-leave{
                 opacity: 0;
                 transform: translate3d(24px,0,0);
                 transform: rotate(180deg);
              }             
            }
    }
    .cart-count{
        display: inline-block;
        vertical-align: top;
        font-size: 10px;
        width: 12px;
        padding-top: 6px;
        line-height: 24px;
        text-align: center;
        color: rgb(147,153,159);
    }
    .cart-add{
        display: inline-block;
        padding: 6px;
        font-size: 24px;
        line-height: 24px;
        color: rgb(0,160,220);

    }
}
</style>
