<template>
    <div class="top">
        <div class="content-wrapper">
            <div class="avatar">
                <img :src="seller.avatar" width="64" height="64" >
            </div>
            <div class="content">
                <div class="title">
                    <span class="brand"></span>
                    <span class="name">{{seller.name}}</span>
                </div>
                <div class="description">
                    {{seller.description}}/{{seller.deliveryTime}}分钟送达
                </div>
                <div v-if="seller.supports" class="support">
                    <span class="icon" :class="classMap[seller.supports[0].type]"></span>
                    <span class="text">{{seller.supports[0].description}}</span>
                </div>
            </div>
            <div v-if="seller.supports" class="support-count">
                <span class="count">{{seller.supports.length}}个</span>
                <i class="icon-keyboard_arrow_right"></i>
            </div>
        </div>
        <div class="bulletin-wrapper" @click="showDetail">
            <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
            <i class="icon-keyboard_arrow_right"></i>

        </div>
        <div class="background">
            <img :src="seller.avatar" width="100%" height="100%" >
        </div>
        <div v-show="detailShow" class="detail" transition="fade">
            <div class="detail-wrapper clearfix">
                <div class="detail-main">
                    <div class="name">{{seller.name}}</div>
                    <div class="star-wrapper">
                       <star :size="48" :score="seller.score"></star>
                    </div>
                    <div class="title">
                        <div class="line"></div>
                        <div class="text">优惠信息</div>
                        <div class="line"></div>
                    </div>
                    <ul class="supports" v-if="seller.supports">
                        <li v-for="item in seller.supports" :key="item.index" class="supports-item" >
                            <span :class="classMap[seller.supports[0].type]" class="icon"></span>
                            <span class="text">{{item.description}}</span>
                        </li>
                    </ul>
                    <div class="title">
                        <div class="line"></div>
                        <div class="text">商家公告</div>
                        <div class="line"></div>
                    </div>
                    <div class="bulletin">
                        <p class="content">{{seller.bulletin}}</p>
                    </div>
                </div>
            <div class="detail-close"  @click="hideDetail">
                <i class="icon-close"></i>
            </div>
        </div>
    </div>
  </div>
</template>

<script>
import star from '../../components/star/star.vue'
export default {
  props: {
    seller: {
      type: Object
    }
  },
  components: {
    star: star
  },
  data () {
    return {
      detailShow: false
    }
  },
  methods: {
    showDetail () {
      this.detailShow = true
    },
    hideDetail () {
      this.detailShow = false
    }
  },
  created () {
    this.classMap = ['decresae', 'discount', 'special', 'invoice', 'guarantee']
  }
}
</script>
icon
<style lang='less' scoped>
@import "../../common/less/_mixins.less";
@import "../../common/less/_base.less";
@import "../../common/less/_icon.less";

.top{
  position: relative;
  color: #fff;
  background:rgba(7,17,27,0.3);
  overflow: hidden;
  .content-wrapper{
      position: relative;
   padding: 24px 12px 18px 24px;
   font-size: 0;
   .avatar{
       display: inline-block;
       vertical-align: top;
       img{
           border-radius: 2px;
       }
   }
   .content{
       display: inline-block;
       margin-left: 16px;
       .title{
           margin:2px 0 8px 0;
           .brand{
               display: inline-block;
               vertical-align: top;
               width: 30px;
               height: 18px;
              // .bg-image('brand');
              background-image: url(brand@2x.png);
               background-size: 30px 18px;
               background-repeat: no-repeat;
           }
           .name{
               margin-left: 6px;
               font-size: 16px;
               line-height: 18px;
               font-weight: bold;
           }
       }
       .description{
           margin-bottom: 10px;
           line-height: 12px;
           font-size: 12px;
       }
       .support{
           .icon{
               display: inline-block;
               margin-right: 4px;
               vertical-align: top;
               width: 12px;
               height: 12px;
               background-size: 12px 12px;
               background-repeat: no-repeat;
               &.decresae{
                   background-image: url(decrease_1@2x.png);
               }
               &.discount{
                   background-image: url(discount_1@2x.png);
               }
               &.guarantee{
                   background-image: url(guarantee_1@2x.png);
               }
               &.invoice{
                   background-image: url(invoice_1@2x.png);
               }
               &.special{
                   background-image: url(special_1@2x.png);
               }
           }
           .text{
               line-height: 12px;
               font-size: 12px;
           }
       }
   }
   .support-count{
       position: absolute;
       bottom: 14px;
       right: 12px;
       padding: 0 8px;
       height: 24px;
       line-height: 24px;
       border-radius: 14px;
       background:rgba(0,0,0,0.2);
       text-align:center;
       .count{
            vertical-align: top;
          font-size: 10px;}
       .icon-keyboard_arrow_right{
           margin-left: 2px;
           font-size: 10px;
           line-height: 24px;}
   }
  }
  .bulletin-wrapper{
      position: relative;
      background: rgba(7,17,27, 0.2);
      line-height: 28px;
      height: 28px;
      padding: 0 22px 0 12px;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
      .bulletin-title{
          display:inline-block;
          vertical-align: middle;
          width: 22px;
          height: 12px;
          background-image: url(bulletin@2x.png);
          background-size: 22px 12px;
          background-repeat: no-repeat;
          }
      .bulletin-text{
          margin:0 4px;
          font-size: 10px;
          }
      .icon-keyboard_arrow_right{
         position: absolute;
         font-size: 10px;
         right: 12px;
         top:10px
          }
  }
  .background{
    position: absolute;
    top:0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: -1;
    filter: blur(10px);
  }
  .detail{
      position: fixed;
      top: 0;
      left: 0;
      width:100%;
      height: 100%;
      overflow: auto;
      z-index: 100;
      background:rgba(7,17,27,0.8);
      transition: all 0.5s;
      .detail-wrapper{
         width: 100%;
         min-height: 100%;
          .detail-main{
            margin-top: 64px;
            padding-bottom: 64px;
            .name{
                 font-size: 16px;
                 line-height:16px;
                 text-align: center;
                 font-weight: 700;
            }
            .star-wrapper{
                margin-top: 18px;
                padding: 2px 0;
                text-align: center;
            }
            .title{
                display: flex;
                width: 80%;
                margin: 28px auto 24px;
                .line{
                    flex:1;
                    position: relative;
                    top:-6px;
                    border-bottom: 1px solid rgba(255,255,255,0.2)

                }
                .text{
                    padding: 0 12px;
                    font-size: 14px;
                    font-weight: 700;
                }
            }
            .supports{
                width: 80%;
                margin: 0 atuo;
                .supports-item{
                    padding: 0 12px;
                    margin-bottom: 12px;
                    font-size: 0;
                    &:last-child{
                        margin-bottom: 0;
                    }
                    .icon{
                        display:inline-block;
                        width:12px ;
                        height: 12px;
                        vertical-align: top;
                        margin-right: 4px;
                        background-size: 12px 12px;
                        background-repeat: no-repeat;
                         &.decresae{
                          background-image: url(decrease_2@2x.png);
                          }
                         &.discount{
                          background-image: url(discount_2@2x.png);
                         }
                        &.guarantee{
                          background-image: url(guarantee_2@2x.png);
                         }
                        &.invoice{
                          background-image: url(invoice_2@2x.png);
                         }
                        &.special{
                          background-image: url(special_2@2x.png);
                         }
                    }
                    .text{
                        line-height: 12px;
                        font-size: 12px;
                    }
                }

            }
            .bulletin{
                width: 80%;
                margin: 0 auto;
                .content{
                    padding: 0 12px;
                    line-height: 24px;
                    font-size: 12px;
                }
            }
          }
      }
      .detail-close{
          position: relative;
          width: 32px;
          height: 32px;
          margin: -64px auto 0 ;
          clear: both;
          font-size: 32px;
      }
  }
}
</style>
