<template>
    <div class="cartcontrol">
        <transition name="move">
           <div class="cart-decrease icon-remove_circle_outline" @click="decreaseCart" v-show="food.count >0" >
           </div>
        </transition>
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
    .move-enter-active, .move-leave-active {
      transition: all 0.3s linear;
      opacity: 1;
      transform: translate3d(0,0,0);
      transform: rotate(0);
     }
    .move-enter, .move-leave-to  {
     opacity: 0;
     transform: translate3d(48px,0,0);
     transform: rotate(180deg);
   }
    .cart-decrease{
        display: inline-block;
        padding: 6px;
        font-size: 24px;
        line-height: 24px;
        color: rgb(0,160,220);
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
