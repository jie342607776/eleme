<template>
  <div class="shopcart" >
        <div class="content" @click="toggleList">
            <div class="content-left">
              <div class="logo-wrapper">
                    <div class="logo" :class="{'highlight':totalCount>0}">
                        <i class="icon-shopping_cart" :class="{'highlight':totalCount>0}"></i>
                    </div>
                    <div class="num" v-if="totalCount">{{totalCount}}</div>
              </div>
              <div class="price" :class="{'highlight':totalCount>0}">￥{{totalPrice}}</div>
              <div class="desc">另需配送费￥{{deliveryPrice}}元</div>
          </div>
          <div class="content-right">
              <div class="pay" :class="{'highlight': totalPrice >= minPrice}">{{payDesc}}</div>
          </div>
        </div>
         <div class="ball-container" v-for="(ball,index) in balls" :key="index">
          <transition name="drop" @before-enter="beforeEnter" @enter="enter" @after-enter="afterEnter">
                <div v-show="ball.show" class="ball">
                    <div class="inner inner-hook"></div>
                </div>
          </transition>
        </div>
        <transition name="fold">
            <div class="shopcart-list" v-show="listShow">
                <div class="list-header">
                    <h1 class="title">购物车</h1>
                    <span class="empty" @click="empty">清空</span>
                </div>
                <div class="list-content" ref="listContent">
                    <ul>
                        <li class="food border-1px" v-for="(food, index) in selectFoods" :key="index">
                            <span class="name">{{food.name}}</span>
                            <div class="price">
                                <span>￥{{food.price*food.count}}</span>
                            </div>
                            <div class="cartcontrol-wrapper">
                                <cartcontrol :food="food"></cartcontrol>
                            </div>
                        </li>
                    </ul>
                </div>
            </div>
        </transition>
        <transition name="fade">
            <div class="list-mask" v-show="listShow"></div>
        </transition>
    </div>
</template>

<script type="text/ec,ascript-6">
import cartcontrol from '../cartcontrol/cartcontrol.vue';
import BScroll from 'better-scroll';
    export default {
        data() {
            return {
                balls: [
                    {
                        show: false
                    },
                    {
                        show: false
                    },
                    {
                        show: false
                    },
                    {
                        show: false
                    },
                    {
                        show: false
                    }
                ],
                dropsBalls: [],
                fold: true
            };
        },
        props: {
            selectFoods: {
                type: Array,
                default() {
                    return [];
                }
            },
            deliveryPrice: {
                type: Number,
                default: 0
            },
            minPrice: {
                type: Number,
                default: 0
            }
        },
        computed: {
            totalPrice() {
                let total = 0;
                this.selectFoods.forEach((food) => {
                    total += food.price * food.count;
                });
                return total;
            },
            totalCount() {
                let count = 0;
                this.selectFoods.forEach((food) => {
                    count += food.count;
                });
                return count;
            },
            payDesc() {
                if (this.totalPrice === 0) {
                    return '￥' + this.minPrice + '起送';
                } else if (this.totalPrice < this.minPrice) {
                    return '还差￥' + (this.minPrice - this.totalPrice) + '起送';
                } else if (this.totalPrice >= this.minPrice) {
                    return '去结算';
                }
            },
            listShow() {
                /* eslint-disable */
                if (!this.totalCount) {
                    this.fold = true;
                    return false;
                }
                let show = !this.fold;
                if (show) {
                    this.$nextTick(() => {
                        if (!this.scroll) {
                            this.scroll = new BScroll(this.$refs.listContent,{
                                click: true
                            })
                        } else {
                            this.scroll.refresh();
                        }
                    })
                }
                return show;
            }
        },
        methods: {
            drop(el) {
                console.log(el);
                for (let i = 0; i < this.balls.length; i++) {
                    let ball = this.balls[i];
                    if (!ball.show) {
                        ball.show = true;
                        ball.el = el;
                        this.dropsBalls.push(ball);
                        return;
                    }
                }
            },
            beforeEnter(el) {
                let count = this.balls.length;
                while (count--) {
                    let ball = this.balls[count];
                    if (ball.show) {
                        let rect = ball.el.getBoundingClientRect();
                        let x = rect.left - 32;
                        let y = -(window.innerHeight - rect.top - 22);
                        el.style.display = '';
                        el.style.webkitTransform = `translate3d(0, ${y}px, 0)`;
                        el.style.transform = `translate3d(0, ${y}px, 0)`;
                        let inner = el.getElementsByClassName('inner-hook')[0];
                        inner.style.webkitTransform = `translate3d(${x}px, 0, 0)`;
                        inner.style.transform = `translate3d(${x}px, 0, 0)`;
                    }
                }
            },
            enter(el) {
                /* eslint-disable no-unused-vars */
                let rf = el.offsetHeight;
                this.$nextTick(() => {
                    el.style.webkitTransform = 'translate3d(0, 0, 0)';
                    el.style.transform = 'translate3d(0, 0, 0)';
                    let inner = el.getElementsByClassName('inner-hook')[0];
                    inner.style.webkitTransform = 'translate3d(0, 0, 0)';
                    inner.style.transform = 'translate3d(0, 0, 0)';
                });
            },
            afterEnter(el) {
                let ball = this.dropsBalls.shift();
                if (ball) {
                    el.style.display = 'none';
                    ball.show = false;
                }
            },
            toggleList() {
                if (!this.totalCount) {
                    return;
                }
                this.fold = !this.fold;
            },
            empty() {
                this.selectFoods.forEach((food) =>{
                    food.count = 0;
                })
            }
        },
        components: {
            cartcontrol
        }
    };
</script>

<style lang="stylus" rel="stylesheet/sylus">
@import "../../commom/stylus/mixin.styl"
    .shopcart
        position fixed
        bottom 0
        left 0
        z-index 50
        background #141d27
        width 100%
        height 48px
        .list-mask
            position fixed
            top 0
            left 0
            width 100%
            height 100%
            z-index -2
            backdrop-filter blur(10px)
            background rgba(7, 17, 27, 0.6)
            &.fade-enter-active,&.fade-enter-active
                transition 0.5s all
            &.&.fade-enter-active
                opacity 1
                background rgba(7, 17, 27, 0.6)
            &.fade-enter,&.fade-leave-to
                opacity 0
                background rgba(7, 17, 27, 0)
        .content
            display flex
            background #141d27
            font-size 0
            .content-left
                flex 1
                .logo-wrapper
                    display inline-block
                    position relative
                    top -10px
                    margin 0 12px
                    padding 6px
                    height 56px
                    width 56px
                    border-radius 50%
                    background #141d27
                    box-sizing border-box
                    vertical-align top
                    .num
                        position absolute
                        top 0
                        right 0
                        width 24px
                        height 16px
                        line-height 16px
                        text-align center
                        font-size 9px
                        color rgb(255, 255, 255)
                        background rgb(240, 20, 20)
                        border-radius 6px
                        font-weight 700
                        box-shadow 0 4px 8px 0 rgba(0, 0, 0, 0.4)
                    .logo
                        width 100%
                        height: 100%
                        border-radius 50%
                        background #2b343c
                        text-align center
                        &.highlight
                            background rgb(0, 160, 220)
                        .icon-shopping_cart
                            font-size 24px
                            color rgba(255, 255, 255, 0.4)
                            line-height 44px
                            &.highlight
                                color #ffffff
                .price
                    display inline-block
                    vertical-align top
                    margin-top 12px
                    line-height 24px
                    padding-right 12px
                    box-sizing border-box
                    border-right 1px solid rgba(255, 255, 255, 0.1)
                    font-size 16px
                    color rgba(255, 255, 255, 0.4)
                    &.highlight
                        color #fff
                .desc
                    display inline-block
                    vertical-align top
                    line-height 24px
                    margin 12px 0 0 12px
                    font-size 12px
                    color rgba(255, 255, 255, 0.4)
            .content-right
                flex 0 0 105px
                width 105px
                .pay
                    height 48px
                    line-height 48px
                    text-align center
                    font-size 12px
                    font-weight 700
                    color rgba(255, 255, 255, 0.4)
                    background #2b343c
                    &.highlight
                        color #ffffff
                        background #00b43c
        .ball-container
            .ball
                position fixed
                left 32px
                bottom 22px
                z-index 2000
                transition all 0.4s cubic-bezier(0.49, -0.29, 0.75, 0.41)
                .inner
                    width 16px
                    height 16px
                    border-radius 59%
                    background rgb(0, 160, 220)
                    transition all 0.4s linear
        .shopcart-list
            position absolute 
            top 0
            left 0 
            z-index -1
            width 100%
            transform translate3d(0, -100%, 0)
            &.fold-leave-active
                 transition all 0.5s
                 transform translate3d(0, -100%, 0)
            &.fold-enter-active
                transition all 0.5s
                transform translate3d(0, -100%, 0)
            &.fold-enter
                transform translate3d(0, 0, 0)
            &.fold-leave-to
                transform translate3d(0, 0, 0)
            .list-header
                height 40px
                line-height 40px
                padding 0 18px
                background #f3f5f7
                border-bottom 1px solid rgba(7, 17, 27, 0.1)
                .title
                    float left
                    font-size 14px
                    color rgb(7, 17, 27)
                .empty
                    float right 
                    font-size 12px
                    color rgb(0, 160, 220)
            .list-content
                padding 0 18px
                max-height 217px
                background #fff
                overflow hidden
                .food
                    position relative
                    padding 12px 0
                    box-sizing border-box
                    border-1px(rgba(7, 17, 27, 0.1))
                .name
                    line-height 24px
                    font-size 14px
                    color rgb(7, 17, 27)
                .price
                    position absolute 
                    right 90px
                    bottom 12px
                    line-height 24px
                    font-size 14px
                    font-weight 700
                    color rgb(240, 20, 20)
                .cartcontrol-wrapper
                    position absolute 
                    right 0
                    bottom 6px
</style>