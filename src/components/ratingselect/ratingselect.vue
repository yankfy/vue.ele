<template>
  <div class="ratingselect">
    <div class="rating-type">
      <span class="block positive" :class="{'active':selectType===2}" @click="select(2,$event)">{{desc.all}}
        <span class="count">{{ratings.length}}</span>
      </span>
      <span class="block positive" :class="{'active':selectType===0}" @click="select(0,$event)">{{desc.positive}}
        <span class="count">{{positives.length}}</span>
      </span>
      <span class="block negative" :class="{'active':selectType===1}" @click="select(1,$event)">{{desc.negative}}
        <span class="count">{{negatives.length}}</span>
      </span>
    </div>
    <div class="switch" :class="{'on':onlyContent}" @click="toggleContent">
      <span class="icon-check_circle"></span>
      <span class="text">只看有内容的评价</span>
    </div>
  </div>
</template>

<script>
import selectType from './selectType'
export default {
  props: {
    ratings: {
      type: Array,
      default() {
        return []
      }
    },
    desc: {
      type: Object,
      default() {
        return {
          all: '全部',
          positive: '满意',
          negative: '不满意'
        }
      }
    }
  },
  data() {
    return {
      selectType: selectType.ALL,
      onlyContent: true
    }
  },
  computed: {
    positives() {
      return this.ratings.filter(rating => {
        return rating.rateType === selectType.POSITIVE
      })
    },
    negatives() {
      return this.ratings.filter(rating => {
        return rating.rateType === selectType.NEGATIVE
      })
    }
  },
  methods: {
    select(type, event) {
      if (!event._constructed) {
        return
      }
      this.selectType = type
      this.$emit('ratingTypeSelect', type)
    },
    toggleContent(event) {
      if (!event._constructed) {
        return
      }
      this.onlyContent = !this.onlyContent
      this.$emit('contentToggle', this.onlyContent)
    }
  }
}
</script>

<style lang="scss" scoped>
@import '../common/scss/mixin';
.ratingselect {
  .rating-type {
    padding: 18px 0;
    margin: 0 18px;
    @include border-1px(rgba(7, 17, 27, 0.1));
    font-size: 0;
    .block {
      display: inline-block;
      padding: 8px 12px;
      margin-right: 8px;
      border-radius: 2px;
      font-size: 12px;
      .count {
        margin-left: 2px;
        font-size: 8px;
        line-height: 16px;
      }
      color: rgb(77, 85, 93);
      &.active {
        color: #ffffff;
      }
      &.positive {
        background-color: rgba(0, 160, 220, 0.2);
        &.active {
          background-color: rgb(0, 160, 220);
        }
      }
      &.negative {
        background-color: rgba(77, 85, 93, 0.2);
        &.active {
          background-color: rgb(77, 85, 93);
        }
      }
    }
  }
  .switch {
    padding: 12px 18px;
    line-height: 24px;
    @include border-1px(rgba(7, 17, 27, 0.1));
    color: rgb(147, 153, 159);
    font-size: 0;
    &.on {
      .icon-check_circle {
        color: #00c850;
      }
    }
    .icon-check_circle {
      margin-right: 4px;
      font-size: 24px;
      display: inline-block;
      vertical-align: top;
    }
    .text {
      font-size: 12px;
    }
  }
}
</style>