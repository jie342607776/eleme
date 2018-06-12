<template>
  <div id="cartcontrol">
      <div class="cart-decrease icon-remove_circle_outline" @click.stop.prevent="decreaseCart" v-if="food.count>0">
      </div>
      <div class="cart-count" v-if="food.count>0">{{food.count}}</div>
      <div class="cart-add icon-add_circle" @click.stop.prevent="addCart"></div>
  </div>
</template>

<script>
import Vue from 'vue';
    export default {
        props: {
            food: {
                type: Object
            }
        },
        methods: {
            addCart() {
                if (!this.food.count) {
                    Vue.set(this.food, 'count', 1);
                } else {
                    this.food.count++;
                }
                this.$emit('cartAdd', event.target);
            },
            decreaseCart() {
                this.food.count--;
            }
        },
        created() {
        }
    };
</script>

<style lang="stylus" rel="stylesheet/stylus">
    #cartcontrol
        font-size 0
        .cart-decrease, .cart-add
            display: inline-block
            padding 6px
            font-size 24px
            line-height 24px
            color rgb(0, 160, 220)
        .cart-count
            display inline-block
            font-size 10px
            color rgb(147, 153, 159)
            width 12px
            vertical-align top
            padding-top 6px
            text-align center
            line-height 24px
</style>
