<template>
  <div class="star" :class="starType">
      <div v-for="(itemClass, index) in itemClasses" :key="index" :class="itemClass" class="star-item"></div>
  </div>
</template>

<script type="text/ec,ascript-6">
    const LENGTH = 5;
    const CLS_ON = 'on';
    const CLS_OFF = 'off';
    const CLS_HALF = 'half';

    export default {
        props: {
            size: {
                type: Number
            },
            score: {
                type: Number
            }
        },
        computed: {
            starType() {
                return 'star-' + this.size;
            },
            itemClasses() {
                let result = [];
                let score = Math.floor(this.score * 2) / 2;
                let hasDecimal = score % 1 !== 0;
                let interger = Math.floor(score);
                for (let i = 0; i < interger; i++) {
                    result.push(CLS_ON);
                }
                if (hasDecimal) {
                    result.push(CLS_HALF);
                }
                while (result.length < LENGTH) {
                    result.push(CLS_OFF);
                }
                return result;
            }
        }
    };
</script>

<style lang="stylus" rel="stylesheet/sylus">
@import "../../commom/stylus/mixin.styl"
    .star
        font-size 0
        .star-item
            display: inline-block
            background-repeat no-repeat
        &.star-24
            .star-item
                width: 10px
                height: 10px
                margin-right 3px
                background-size 10px 10px
                &.on
                    bg-image('star24_on')
                &.off
                    bg-image('star24_off')
                &.half
                    bg-image('star24_half')
        &.star-36
            .star-item
                width: 15px
                height: 15px
                margin-right 6px
                background-size 15px 15px
                &.on
                    bg-image('star36_on')
                &.off
                    bg-image('star36_off')
                &.half
                    bg-image('star36_half')
        &.star-48
            .star-item
                width: 20px
                height: 20px
                margin-right 22px
                background-size 20px 20px
                &.on
                    bg-image('star48_on')
                &.off
                    bg-image('star48_off')
                &.half
                    bg-image('star48_half')
</style>
