<template>
    <div class="goods">
        <div class="menu-wrapper" ref="menuwrapper">
            <ul>
                <li v-for="item in goods" :key="item.id" class="menu-item"  :class="{'current':currentIndex === item.id}"
                @click="selectMenu(item.id,$event)">
                    <span class="text">
                        <span v-if="item.type > 0" class="icon" :class="classMap[item.type]"></span>
                        {{item.name}}</span>
                </li>
            </ul>
        </div>
        <div class="foods-wrapper" ref="foodswrapper">
            <ul>
                <li v-for="item in goods" :key="item.index" class="food-list food-list-hook">
                    <h1 class="title">{{item.name}}</h1>
                    <ul>
                        <li   v-for="food in item.foods" :key="food.index" class="food-item">
                            <div class="icon" @click="selectFood(food,$event)">
                                <img :src="food.icon" width="57px" height="57px">
                            </div>
                            <div class="content">
                                <h2 class="name">{{food.name}}</h2>
                                <p class="desc">{{food.description}}</p>
                                <div class="extra">
                                    <span class="count">月销{{food.sellCount}}份</span>
                                    <span>好评率{{food.rating}}%</span>
                                </div>
                                <div class="price">
                                    <span class="now">￥{{food.price}}</span>
                                    <span v-show="food.oldPrice" class="old">￥{{food.oldPrice}}</span>
                                </div>
                                <div class="cartcontrol-wrapper">
                                  <cartcontrol :food="food"></cartcontrol>
                                </div>
                            </div>
                        </li>
                    </ul>
                </li>
            </ul>
        </div>
        <shopcart ref='shopcart' :deliveryPrice="seller.deliveryPrice" :minPrice="seller.minPrice"
        :select-foods="selectFoods"></shopcart>
        <food  :food="selectedFood" ref='food'></food>
    </div>
</template>

<script>
import BScroll from 'better-scroll'
import shopcart from '../../components/shopcart/shopcart'
import cartcontrol from '../../components/cartcontrol/cartcontrol'
import food from '../../components/food/food'
export default {
  data () {
    return {
      goods: [],
      ListHeight: [],
      scrollY: 0,
      selectedFood: {}
    }
  },
  components: {
    shopcart: shopcart,
    cartcontrol: cartcontrol,
    food: food
  },
  props: {
    seller: {
      type: Object
    }
  },
  created () {
    this.classMap = ['decresae', 'discount', 'special', 'invoice', 'guarantee']
    this.$axios('/api/goods').then((response) => {
      if (response.data.erron === 0) {
        this.goods = response.data.data
        this.$nextTick(() => {
          this.initScroll()
          this.calculateHeight()
        })
        console.log(this.goods)
      }
    })
  },
  computed: {
    currentIndex () {
      for (let i = 0; i < this.ListHeight.length; i++) {
        let height1 = this.ListHeight[ i ]
        let height2 = this.ListHeight[ i + 1 ]
        if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
          return i
        }
      }
      return 0
    },
    selectFoods () {
      let foods = []
      this.goods.forEach(good => {
        good.foods.forEach((food) => {
          if (food.count) {
            foods.push(food)
          }
        })
      })
      return foods
    }
  },
  methods: {
    selectFood (food, event) {
      if (!event._constructed) {
        return
      }
      this.selectedFood = food
      this.$refs.food.show()
    },
    selectMenu (id, event) {
      if (!event._constructed) {
        return
      }
      console.log(id)
    },
    initScroll () {
      this.menuScroll = new BScroll(this.$refs.menuwrapper, {
        click: true
      })
      this.foodsScroll = new BScroll(this.$refs.foodswrapper, {
        click: true,
        probeType: 3
      })
      this.foodsScroll.on('scroll', (pos) => {
        this.scrollY = Math.abs(Math.round(pos.y))
      })
    },
    calculateHeight () {
      let foodList = this.$refs.foodswrapper.getElementsByClassName('food-list-hook')
      let height = 0
      this.ListHeight.push(height)
      for (let i = 0; i < foodList.length; i++) {
        let item = foodList[i]
        height += item.clientHeight
        this.ListHeight.push(height)
      }
    }
  }
}
</script>
<style lang="less" scoped>
.goods{
    display: flex;
    position: absolute;
    width: 100%;
    top:174px;
    bottom: 46px;
    overflow: hidden;
    .menu-wrapper{
        flex: 0 0 80px;
        width: 80px;
        background: #f3f5f7;
        .menu-item{
            display: table;
            height: 54px;
            width: 56px;
            line-height: 14px;
            padding: 0 12px;
        .current{
                position: relative;
                margin-top: -1px;
                z-index: 10;
                background:#fff;
                font-size: 700;
                .text{
                    border-bottom: none;
                }
            }
            .icon{
               display:inline-block;
               width:12px ;
               height: 12px;
               vertical-align: top;
               margin-right: 2px;
               background-size: 12px 12px;
               background-repeat: no-repeat;
               &.decresae{
                    background-image: url(decrease_3@2x.png);
                }
               &.discount{
                    background-image: url(discount_3@2x.png);
                }
               &.guarantee{
                    background-image: url(guarantee_3@2x.png);
                }
               &.invoice{
                    background-image: url(invoice_3@2x.png);
                }
               &.special{
                    background-image: url(special_3@2x.png);
                }
            }
            .text{
                display: table-cell;
                width: 56px;
                font-size: 12px;
                vertical-align: middle;
                border-bottom: 1px  solid rgba(7,17,27,0.1);
            }
          }
        }
    }
    .foods-wrapper{
        flex: 1;
        .title{
            padding-left: 14px;
            font-size: 12px;
            line-height: 26px;
            height:26px ;
            border-left: 2px solid #d9dde1;
            color:rba(147,153,159) ;
            background: #f3f5f7;
        }
        .food-item{
            display: flex;
            margin: 18px;
            padding-bottom: 18px;
            border-bottom: 1px  solid rgba(7,17,27,0.1);
            &:last-child{
                border-bottom:none;
                margin-bottom: 0;
            }
            .icon{
                flex:0 0 57px;
                margin-right: 10px;
            }
            .content{
                position: relative;
                width: 100%;
                height: 100%;
                flex: 1;
                .name{
                    margin:2px 0 8px 0;
                    line-height: 14px;
                    height: 14px;
                    font-size: 14px;
                    color: rgb(7,17,27);
                }
                .desc{
                    margin-bottom: 8px;
                    color:rba(147,153,159);
                    font-size: 10px;
                    line-height: 10px;
                }
                .extra{
                    color:rba(147,153,159);
                    font-size: 10px;
                    line-height: 10px;
                    .count{
                        margin-right: 12px;
                    }
                }
                .price{
                    font-weight: 700;
                    line-height: 24px;
                    .now{
                        color: rgb(240,20,20);
                        font-size: 14px;
                        margin-right: 8px;
                    }
                    .old{
                        font-size: 10px;
                        text-decoration: line-through;
                        color: rgb(147,153,159);

                    }
                }
                .cartcontrol-wrapper{
                  position: absolute;
                  right: 0;
                  bottom: -6px;

                }
            }
        }
    }
</style>
