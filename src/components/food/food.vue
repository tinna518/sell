<template>
    <div class="food" v-show="showFlag"  translate="move" ref="food">
      <div class="food-content">
        <div class="image-header">
          <img :src="food.image">
          <div class="back" @click="hide">
            <i class="icon-arrow_lift"></i>
          </div>
        </div>
        <div class="content">
          <div class="title">{{food.name}}</div>
          <div class="detail">
            <span class="sell-count">月售{{food.sellCount}}份</span>
            <span class="rating">好评率{{food.rating}}%</span>
          </div>
          <div class="price">
            <span class="new">￥{{food.price}}</span>
            <span class="old" v-if="food.oldPrice">￥{{food.oldPrice}}</span>
          </div>
          <div class="cartcontrol-wrapper">
            <cartcontrol :food=food></cartcontrol>
          </div>
          <div class="buy" @click="addFrist" v-show="!food.count ||food.count ===0">加入购物车</div>
        </div>
        <split></split>
        <div class="info" v-show="food.info">
          <h1 class="title" >商品信息</h1>
          <p class="text">{{food.info}}</p>
        </div>
        <split></split>
      </div>
    </div>
</template>

<script>
import BScroll from 'better-scroll'
import cartcontrol from '../../components/cartcontrol/cartcontrol'
import split from '../../components/split/split'
import Vue from 'vue'
export default {
  props: {
    food: {
      type: Object
    }
  },
  data () {
    return {
      showFlag: false
    }
  },
  components: {
    cartcontrol: cartcontrol,
    split: split
  },
  methods: {
    addFrist () {
      console.log('c')
      if (!this.food.count) {
        Vue.set(this.food, 'count', 1)
      }
    },
    show () {
      this.showFlag = true
      this.$nextTick(() => {
        this.scroll = new BScroll(this.$refs.food, {
          click: true
        })
      })
    },
    hide () {
      this.showFlag = false
    }
  }
}
</script>
<style lang="less" scoped>
@import "../../common/less/_icon.less";
    .food{
        position: fixed;
        top: 0;
        left: 0;
        bottom: 48px;
        z-index: 30;
        width: 100%;
        background: #fff;
        &.move-transition{
          transition: all 0.2s linear;
          transform: translate3d(0 0 0);
        }
        &.move-enter,&.move-leave{
          transform: translate3d(100% 0 0);
        }
        .food-content{
          .image-header{
            position: relative;
            width: 100%;
            height: 0;
            padding-top: 100%;
            img{
              position: absolute;
              top: 0;
              left: 0;
              width: 100%;
              height: 100%;
            }
            .back{
              position:absolute;
              top: 10px;
              left: 0;
              .icon-arrow_lift{
                display: block;
                padding: 10px;
                font-size: 20px;
                color: #fff;
              }

            }
          }
        .content{
           position: relative;
          padding: 18px;
          .title{
            font-size: 14px;
            line-height: 14px;
            font-weight: 700;
            color:rgb(7,17,27);
            margin-bottom: 8px;
          }
          .detail{
            font-size: 0;
            line-height: 10px;
            height: 10px;
            color: rgb(147, 153, 159);
            margin-bottom: 18px;
            .sell-count{
              font-size: 10px;
              margin-right: 12px;
            }
            .rating{
              font-size: 10px;
            }
          }
          .price{
            line-height: 24px;
            font-weight: 700;
            .new{
              color:rgb(240,20,20);
              font-size: 14px;
              margin-right: 8px;
            }
            .old{
              color: rgb(147, 153, 159);
               font-size: 10px;
               text-decoration: line-through;
            }
          }
          .cartcontrol-wrapper{
            position: absolute;
            right: 12px;
            bottom: 12px;
          }
          .buy{
            position: absolute;
            right: 18px;
            bottom: 18px;
            z-index: 10;
            font-size: 10px;
            height: 24px;
            padding: 0 12px;
            line-height: 24px;
            border-radius:12px ;
            color: #fff;
            background: rgb(0,160,220);
            box-sizing: border-box;
          }
        }
        .info{
          padding: 18px;
          .title{
            font-size: 14px;
            line-height: 14px;
            font-weight: 700;
            color:rgb(7,17,27);
            margin-bottom: 6px;
          }
          .text{
            font-size: 12px;
            line-height: 24px;
            color: rgb(77,85,93);
            padding: 0 8px;
            font-weight: 200;
          }
        }
    }
  }
</style>
