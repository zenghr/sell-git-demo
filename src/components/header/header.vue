<template>
  <div class="header">
    <div class="content-wrapper">
      <div class="avatar">
        <img width="64" height="64" :src="seller.avatar" alt="">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div class="support" v-if="seller.supports">
          <span class="icon" :class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div class="support-count" v-if="seller.supports" @click="showDetail()">
        <span class="count">{{seller.supports.length}}个</span>
        <i class="icon-keyboard_arrow_right"></i>
      </div>
    </div>
    <div class="bulletin-wrapper">
      <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
      <i class="icon-keyboard_arrow_right" @click="showDetail()"></i>
    </div>
    <div class="backgroundImg">
      <img width="100%" height="100%" :src="seller.avatar" alt="">
    </div>
    <transition name="fade">
      <div v-show="detailShow" class="detail">
        <div class="detail-wrapper clearfix">
          <div class="detail-main">
            <h1>{{seller.name}}</h1>
            <div class="star-wrapper">
              <star :size="48" :score="seller.score"></star>
            </div>
            <div class="title">
              <div class="line"></div>
              <h2>优惠信息</h2>
              <div class="line"></div>
            </div>
            <ul v-if="seller.supports" class="supports">
              <li class="support-item" v-for="item in seller.supports">
                <span class="icon" :class="classMap[item.type]"></span>
                <span class="text">{{item.description}}</span>
              </li>
            </ul>
            <div class="title">
              <div class="line"></div>
              <h2>商家公告</h2>
              <div class="line"></div>
            </div>
            <div class="notice">{{seller.bulletin}}</div>
          </div>
        </div>
        <div class="detail-close">
          <i class="icon-close" @click="detailShow=false"></i>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
  import star from '../star/star.vue';

  export default {
    name: 'header',
    props: {
      seller: {
        type: Object
      }
    },
    components: {
      star
    },
    data() {
      return {
        detailShow: false
      };
    },
    methods: {
      showDetail() {
        this.detailShow = true;
      }
    },
    created() {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import '../../common/stylus/mixin.styl';

  .header
    color: #ffffff
    font-size: 0
    position: relative
    background: rgba(7, 17, 27, 0.5)
    overflow: hidden
    .content-wrapper
      position: relative
      padding: 24px 12px 18px 24px
      .avatar
        display: inline-block
        vertical-align: top
        img
          -webkit-border-radius: 4px
          -moz-border-radius: 4px
          border-radius: 4px
      .content
        display: inline-block
        vertical-align: top
        padding: 2px 0 0 16px
        .title
          font-size: 0
          .brand
            display: inline-block
            vertical-align: top
            width: 30px
            height: 18px
            bg-image("brand")
            /*background-image: url("brand@2x.png")*/
            background-size: 30px 18px
            background-repeat: no-repeat
          .name
            margin-left: 6px
            font-size: 16px
            color: rgb(255, 255, 255)
            font-weight: bold
            line-height: 18px
        .description
          font-size: 12px
          line-height: 12px
          margin: 8px 0 10px 0
        .support
          .icon
            display: inline-block
            vertical-align: top
            width: 12px
            height: 12px
            &.decrease
              bg-image('decrease_1')
            &.discount
              bg-image('discount_1')
            &.guarantee
              bg-image('guarantee_1')
            &.invoice
              bg-image('invoice_1')
            &.special
              bg-image('special_1')
            /*background-image: url("decrease_1@2x.png")*/
            background-size: 12px 12px
            background-repeat: no-repeat
          .text
            font-size: 10px
            line-height: 12px
            margin-left: 4px
      .support-count
        position: absolute
        right: 12px
        bottom: 14px
        line-height: 24px
        padding: 0 8px
        height: 24px
        border-radius: 14px
        background: rgba(0, 0, 0, 0.2)
        text-align: center
        .count
          font-size: 10px
        .icon-keyboard_arrow_right
          font-size: 10px
          margin-left: 2px
          line-height: 24px
    .bulletin-wrapper
      height: 28px
      line-height: 28px
      padding: 0 24px 0 12px
      white-space: nowrap
      overflow: hidden
      text-overflow: ellipsis
      font-size: 10px
      position: relative
      background-color: rgba(7, 17, 27, 0.2)
      .bulletin-title
        display: inline-block
        vertical-align: middle
        width: 22px
        height: 12px
        bg-image('bulletin')
        background-size: 22px 12px
        background-repeat: no-repeat
      .bulletin-text
        margin: 0 4px
        vertical-align: middle
      .icon-keyboard_arrow_right
        position: absolute
        right: 12px
        top: 10px
    .backgroundImg
      position: absolute
      left: 0
      top: 0
      z-index: -1
      width: 100%
      height: 100%
      filter: blur(10px)
    .detail
      position: fixed
      left: 0
      top: 0
      z-index: 100
      width: 100%
      height: 100%
      overflow: auto
      font-size: 24px
      background: rgba(7, 17, 27, 0.8)
      backdrop-filter: blur(10px)
      &.fade-enter-active, &.fade-leave-active
        transition: opacity .5s
      &.fade-enter, &.fade-leave-to
        opacity: 0
      .detail-wrapper
        width: 100%
        min-height: 100%
        .detail-main
          padding: 64px 36px
          text-align: center
          h1
            font-size: 16px
            font-weight: 700
            line-height: 16px
          .star-wrapper
            margin-top: 16px
          .title
            display: flex
            width: 100%
            margin: 28px 0 24px 0
            .line
              position: relative
              top: -7px
              flex: 1
              border-bottom: 1px solid rgba(255, 255, 255, 0.2)
            h2
              padding: 0 12px
              font-size: 14px
              font-weight: 700
              line-height: 14px

          .supports
            font-size: 0
            .support-item
              margin-bottom: 12px
              text-align: left
              .icon
                display: inline-block
                vertical-align: middle
                width: 16px
                height: 16px
                margin: 0 6px 0 12px
                &.decrease
                  bg-image('decrease_2')
                &.discount
                  bg-image('discount_2')
                &.guarantee
                  bg-image('guarantee_2')
                &.invoice
                  bg-image('invoice_2')
                &.special
                  bg-image('special_2')
                /*background-image: url("decrease_1@2x.png")*/
                background-size: 16px 16px
                background-repeat: no-repeat
              .text
                vertical-align: middle
                font-size: 12px
                line-height: 12px
          .notice
            margin: 0 12px
            font-size: 12px
            line-height: 24px
            text-align: left
      .detail-close
        position: relative
        width: 32px
        height: 32px
        font-size: 32px
        margin: -64px auto 0 auto
        clear: both
        color: rgba(255, 255, 255, 0.5)
</style>
