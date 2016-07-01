<template>

  <div class="carousel slide"  @mouseenter="pause()" @mouseleave="start()">

    <!-- 下面圆点nav -->
    <ol class="carousel-indicators" v-show="indicators">
        <li v-for="item in slides" v-bind:class="{'active': item == index}" @click="changeSlide($index)"></li>
    </ol>

    <!-- 图片主体部分 -->
    <div class="carousel-inner" role="listbox">
        <slot></slot>
    </div>

    <!-- 上下切换 -->
    <a class="left carousel-control" href="#" @click.stop.prevent="prev" v-show="controls">
        <span class="icon-prev"></span>
    </a>
    <a class="right carousel-control" href="#" @click.stop.prevent="next" v-show="controls">
        <span class="icon-next"></span>
    </a>

  </div>

</template>

<script>
// 浏览器是否支持css3
import { csstransitions } from '../utils/helpers.js'
// 兼容ie9
import '../utils/ie9_polyfill.js'

// 浏览器不支持过渡则改变动画方式
const TRANSITION_DURATION = csstransitions() ? 600 : 0

const DIRECTION = {
  rtl: {
    outgoing: 'left',
    incoming: 'right',
    overlay: 'next',
  },
  ltr: {
    outgoing: 'right',
    incoming: 'left',
    overlay: 'prev',
  }
}

export default {
  data () {
    return {
      items: [],
      index: 0,
      slidesCount: 0,
      animating: false,
      slides: [],
      direction: DIRECTION.rtl // 从左到右的动画
    }
  },
  props: {
    interval: {
      type: Number,
      default: 3000
    },
    indicators: {
      type: Boolean,
      default: true
    },
    controls: {
      type: Boolean,
      default: true
    }
  },
  methods: {
    // 上一张
    prev () {
      if (this.animating) return
      this.index--
      if (this.index < 0) {
        this.index = this.slidesCount
      }
    },
    // 下一张
    next () {
      if (this.animating) return
      this.index++
      if (this.index > this.slidesCount) {
        this.index = 0
      }
    },
    changeSlide(index) {
      this.index = index
    },
    // 关闭自动轮播
    pause () {
      if (this.interval === 0 || typeof this.interval === 'undefined') return
      clearInterval(this._intervalId)
    },
    // 开启轮播
    start () {
      if (this.interval === 0 || typeof this.interval === 'undefined') return
      this._intervalId = setInterval(() => {
        this.next()
      }, this.interval)
    }
  },
  ready () {
    this.items = this.$el.querySelectorAll('.carousel-item')
    this.slidesCount = this.items.length - 1
    this.slides = Array.apply(null, {length: this.items.length}).map(Number.call, Number)

    // 第一个slide加上class
    this.items[0].classList.add('active')
    this.start()
  },
  watch: {
    index (val, oldVal) {
      this.animating = true
      this.direction = DIRECTION.rtl

      if (val < oldVal) {
        this.direction = DIRECTION.ltr
      }

      this.items[val].classList.add(this.direction.incoming, this.direction.overlay)
      this.items[val].offsetWidth

      this.items[val].classList.add('active')

      this.items[oldVal].classList.add(this.direction.outgoing)
      this.items[val].classList.remove(this.direction.incoming)

      // 动画效果
      this._carouselAnimation = setTimeout(() => {
      this.items[oldVal].classList.remove(this.direction.outgoing, 'active')
      this.items[val].classList.remove(this.direction.overlay)
      this.animating = false

      }, TRANSITION_DURATION)
    }
  },
  destroyed() {
    clearTimeout(this._carouselAnimation)
    clearInterval(this._intervalId)
  }
}

</script>

<style>
  @import "./index.css";
</style>
