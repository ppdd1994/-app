<template>
    <div class="header">
      <div class="content-wrapper">
        <div class="avatar">
          <img width="64" height="64" :src="seller.avatar" alt="avatar">
        </div>
        <div class="content">
          <div class="title">
            <span class="brand"></span>
            <span class="name">{{seller.name}}</span>
          </div>
          <div class="description">
            {{seller.description}}/{{seller.deliveryTime}}分钟送达
          </div>
          <div v-if="seller.supports" class="support" >
            <span class="icon" :class="classMap[seller.supports[0].type]"></span>
            <span class="text">{{seller.supports[0].description}}</span>
          </div>
        </div>
        <div v-if="seller.supports" class="support-count" @click="showDetail">
          <span class="count">{{seller.supports.length}}</span>
          <i class="icon icon-keyboard_arrow_right"></i>
        </div>
      </div>
      <div class="bulletin-wrapper">
        <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
        <i class="icon-keyboard_arrow_right"></i>
      </div>
      <div class="background">
        <img :src="seller.avatar" width="100%" height="100%" alt="">
      </div>
      <div class="detail" v-show="detailShow" transition="fade">
        <div class="detail-wrapper clearfix">
          <div class="detail-main">
            <h1>{{seller.name}}</h1>
            <div class="star-wrapper">
              <star :size="48" :score="seller.score"></star>
            </div>
            <div class="title">
              <div class="line"></div>
              <div class="text">优惠信息</div>
              <div class="line"></div>
            </div>
            <ul v-if="seller.supports" class="supports">
              <li class="supports-item" v-for="item in seller.supports">
                <span class="icon" :class="classMap[seller.supports[$index].type]">
                </span>
                <span class="text">{{seller.supports[$index].description}}</span>
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
        </div>
        <div class="detail-close">
          <span class="icon-close" @click="hideDetail" ></span>
        </div>
      </div>
    </div>
</template>
<script type="text/ecmascript-6">
  import star from 'components/star/star';
    export default {
        props: {
            seller: {
                type: Object
            }
        },
        data() {
            return {
                detailShow: false
            };
        },
        methods: {
            showDetail() {
                this.detailShow = true;
            },
            hideDetail() {
                this.detailShow = false;
                console.log(this.detailShow);
            }
        },
        created() {
            this.classMap = ['decrease', 'discount', 'special', 'invoice',
            'guarantee'];
        },
        components: {
            'star': star
        }
    };
</script>
<style>
  .header{
    color: #fff;
    position: relative;
    background-color: rgba(7,17,27,0.5);
    overflow: hidden;
  }
  .content-wrapper{
    padding: 24px 12px 18px 24px;
    font-size: 0;
    position: relative;
  }
  .avatar {
    display: inline-block;
    vertical-align: top;
  }
  .avatar img{
    border-radius: 2px;
  }
  .header .content{
    display: inline-block;
    margin-left: 16px ;
  }
  .content .title{
    margin: 2px 0 8px 0;
    line-height: 18px;
    font-size: 16px;
    font-weight: bold;
  }
  .content .title .brand{
    display: inline-block;
    width: 30px;
    height: 18px;
    vertical-align: top;
    background-image: url("brand@2x.png");
    background-size: 30px 18px;
    background-repeat: no-repeat;
  }
  .content .title .name{
    line-height: 18px;
  }
  .content .description{
    margin-bottom: 10px;
    font-size: 12px;
  }
  .content .support{
    font-size: 10px;
  }
  .content .support .icon {
    display: inline-block;
    width: 12px;
    height: 12px;
    margin-right: 4px;
    vertical-align: top;
    background-size: 12px 12px;
    background-repeat: no-repeat;
  }

  .decrease{
    background-image: url("decrease_1@2x.png");
  }
  .discount{
    background-image: url("discount_1@2x.png");
  }
  .special{
    background-image: url("special_1@2x.png");
  }
  .invoice{
    background-image: url("invoice_1@2x.png");
  }
  .guarantee{
    background-image: url("guarantee_1@2x.png");
  }
  .content .support .text{
    font-size: 10px;
  }
  .support-count{
    position: absolute;
    right: 12px;
    bottom: 18px;
    padding: 0 8px;
    height: 24px;
    line-height: 24px;
    border-radius: 14px;
    background-color: rgba(0,0,0,0.2);
    text-align: center;

  }
  .support-count .count{
    font-size: 10px;
    margin-right: 3px;
    text-align: center;
  }
  .support-count .icon{
    font-size: 10px;
  }
  .bulletin-wrapper{
    height: 28px;
    line-height: 28px;
    padding: 0 22px 0 12px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    background-color: rgba(7,17,27,0.2);
  }
  .bulletin-wrapper .bulletin-title{
    display: inline-block;
    width: 22px;
    height: 12px;
    vertical-align: middle;
    background-image: url("bulletin@2x.png");
    background-size: 22px 12px;
    background-repeat: no-repeat;
  }
  .bulletin-wrapper .bulletin-text{
    font-size: 10px;
    line-height: 28px;
    margin-left: 4px;
  }
  .bulletin-wrapper i{
    font-size: 10px;
  }
  .header .background{
    position: absolute;
    top:0;
    left: 0;
    width: 100%;
    height:100%;
    z-index: -1;
    filter:blur(10px)
  }
  .header .detail{
    width: 100%;
    height: 100%;
    position: fixed;
    top: 0;
    left: 0;
    z-index: 100;
    overflow: auto;
    transition: all 0.5s;
  }
  .header .detail.fade-transition{
    opacity: 1;
    background: rgba(7,17,27,0.8);
  }
  .header .detail.fade-enter, .header .detail.fade-leave{
    opacity: 0;
    background: rgba(7,17,27,0);
  }
  .header .detail .detail-wrapper{
    min-height: 100%;
    width: 100%;

  }
  .header .detail .detail-wrapper .detail-main{
    margin-top: 64px;
    padding-bottom: 64px;
  }
  .header .detail .detail-close{
    position: relative;
    width: 32px;
    height: 32px;
    margin: -64px auto 0 auto;
    clear: both;
    font-size: 32px;
  }
  .header .detail-main h1{
    line-height: 16px;
    text-align: center;
    font-size: 16px;
    font-weight: 700;
  }
  .header .detail-main .star-wrapper{
    margin-top: 18px;
    padding: 2px 0;
    text-align: center;

  }
  .header .detail-main .title{
    width: 80%;
    margin: 28px auto 24px;
    display: flex;
  }
  .header .title .line{
    flex: 1;
    position: relative;
    top: -6px;
    border-bottom: 1px solid rgba(255,255,255,0.2);

  }
  .header .title .text{
    padding: 0 12px;
    font-size: 14px;
    font-weight: 700;

  }
  .header .supports{
    width: 80%;
    margin: 0 auto;

  }
  .header .supports .supports-item{
    padding: 0 12px;
    margin-bottom: 12px;
    font-size: 0;
  }
  .header .supports .supports-item:last-child{
    margin-bottom: 0;
  }
  .header .supports .supports-item .icon{
    display: inline-block;
    width: 16px;
    height: 16px;
    margin-right: 6px;
    vertical-align: top;
    background-size: 16px 16px;
    background-repeat: no-repeat;

  }
  .header .supports .supports-item .text{
    line-height: 12px;
    font-size: 12px;
  }
  .header .detail-main .bulletin{
    width: 80%;
    margin: 0 auto;
  }
  .header .detail-main .bulletin .content{
    font-size: 12px;
    line-height: 24px;
  }
</style>
