<template>
  <div class='header'>
    <div class="content-wrapper">
      <div class="avatar">
        <img width="64" height="64" :src="seller.avatar">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <div class="name">
            {{seller.name}}
          </div>
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div class="supports" v-if="seller.supports">
          <span class="icon"  :class="classMap[seller.supports[0].type]"></span>
          <span class="text" >
            {{seller.supports[0].description}}
          </span>
        </div>
      </div>
      <div v-if="seller.supports" class="support-count" @click="showDetail()">
        <div class="count">{{seller.supports.length}}个</div>
        <i class="icon-keyboard_arrow_right"></i>
      </div>
    </div>
    <div class="bulletin-wrapper" @click="showDetail()">
      <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
      <i class="icon-keyboard_arrow_right"></i>
    </div>
    <div class="background">
      <img :src="seller.avatar" width="100%" height="100%">
    </div>
    <transition name="fade">
      <div class="detail" v-show="detailShow">
        <div class="detail-background"></div>
        <div class="detail-wrapper clearfix">
          <div class="detail-main">
            <h1 class="name">{{seller.name}}</h1>
            <div class="star-wrapper">
            <star :size="36" :score="seller.score"></star>
            </div>
            <div class="title">
              <div class="line"></div>
              <div class="text">优惠信息</div>
              <div class="line"></div>
            </div>
            <ul v-if="seller.supports" class="supports">
              <li v-for="(item, index) in seller.supports" :key="index" class="support-item" >
                <span :class="classMap[item.type]" class="icon"></span>
                <span class="text">{{item.description}}</span>
              </li>
            </ul>
            <div class="title">
              <div class="line"></div>
              <div class="text">商家公告</div>
              <div class="line"></div>
            </div>
            <p class="bulletin">{{seller.bulletin}}</p>
          </div>
        </div>
        <div class="detail-close">
          <i class="icon-close" @click="closeDetail()"></i>
        </div>
      </div>
    </transition>
  </div>
</template>

<script type="text/ecmascript-6">
import star from '../star/star.vue';

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
      closeDetail() {
        this.detailShow = false;
      }
    },
    created() {
      this.classMap = ['decrease', 'discount', 'guarantee', 'invoice', 'special'];
    },
    components: {
      'star': star
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import "../../commom/stylus/mixin.styl";
  .header
    position relative
    overflow hidden
    color: #ffffff
    background: rgba(7, 17, 27, 0.5)
    .content-wrapper
      position relative
      padding: 24px 12px 18px 24px
      font-size: 0
      .avatar
        display: inline-block
        vertical-align: top
        img
          border-radius: 2px
      .content
        display inline-block
        margin-left: 16px
        .title
          padding: 2px 0 8px 0
          .brand
            display: inline-block
            bg-image('brand')
            height: 18px
            width: 30px
            margin-right 6px
            background-size:30px 18px
            background-repeat: no-repeat
            vertical-align: top
          .name
            display: inline-block
            font-size: 16px
            color: rgb(255,255,255)
            font-weight: bold
        .description
          font-size: 12px
          color: rgb(255,255,255)
          line-height:12px
          margin-bottom: 10px
        .supports
          .icon
            display inline-block
            vertical-align: top
            margin-right 4px
            width: 12px
            height: 12px
            background-size 12px
            background-repeat no-repeat
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
          .text
            font-size: 10px
            line-height: 12px
            color: rgb(255,255,255)
      .support-count
        position: absolute
        right: 12px
        bottom: 14px
        padding: 0 8px
        height: 24px
        line-height: 24px
        border-radius 12px
        background rgba(0, 0, 0, 0.2)
        text-align center
        .count
          display inline-block
          font-size: 10px
          vertical-align: top
        .icon-keyboard_arrow_right
          display inline-block
          line-height:24px
          margin-left 2px
          font-size: 10px
    .bulletin-wrapper
      position relative
      height: 28px
      line-height 28px
      padding 0px 28px 0 12px
      background-color: rgba(7, 17, 27, 0.2)
      white-space: nowrap
      overflow: hidden
      text-overflow: ellipsis
      .bulletin-title
        margin-top 9px
        vertical-align top
        display inline-block
        width: 22px
        height: 12px
        bg-image('bulletin')
        background-size: 22px 12px
        background-repeat no-repeat
      .bulletin-text
      vertical-align top
        font-size 10px
        margin: 0 4px
      .icon-keyboard_arrow_right
        position absolute
        right 12px
        margin-top:10px
        font-size:10px
    .background
      width 100%
      height 100%
      position absolute
      left 0
      top 0
      z-index -1
      filter blur(10px)
    .detail
      position fixed
      z-index 100
      top:0
      right 0
      width 100%
      height 100%
      overflow auto
      backdrop-filter blur(10px)
      opacity 1
      background rgba(7, 17, 27, 0.8)
      &.fade-leave-active,&.fade-enter-active
        transition all 0.5s
      &.fade-enter
        opacity 0
        background rgba(7, 17, 27, 0)
      &.fade-leave-to
        opacity 0
        background rgba(7, 17, 27, 0)
      .detail-wrapper
        width 100%
        min-height 100%
        .detail-main
          margin-top 64px
          padding-bottom 64px
          .name
            font-size 16px
            font-weight: 700
            line-height: 32px
            text-align center
          .star-wrapper
            width 100%
            text-align center
            margin-top 16px
          .title
            display flex
            width:80%
            margin 18px auto
            .line
              flex 1
              position relative
              top -6px
              border-bottom 1px solid rgba(255, 255, 255, 0.2)
            .text
              font-size 14px
              font-weight 700
              line-height 14px
              padding 0 12px
          .supports
            width 80%
            margin 24px auto
            .support-item
              font-size 0
              margin-bottom 12px
              &:last-child
                margin-bottom 0
              .icon
                width 16px
                height 16px
                margin -2px 6px 0 6px
                display inline-block
                vertical-align top
                background-size 16px 16px
                background-repeat no-repeat
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
              .text
                font-size 12px
                font-weight 200
                line-height:12px
          .bulletin
            width 70%
            margin 24px auto 0 auto
            font-size 12px
            font-weight 200
            line-height 24px
      .detail-close
        margin -64px auto 0 auto
        height 32px
        width 32px
        font-size 30px

</style>
