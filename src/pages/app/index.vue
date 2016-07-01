<template>
  <div class="">
    <div slot="markup">
      <!-- Html markup start-->
      <carousel
        :interval="interval",
        :controls="controls"
        :indicators="indicators">
        <slide>
          <img src="../../assets/carousel-one.png" class="img-fluid">
        </slide>
        <slide>
          <img src="../../assets/carousel-two.png" class="img-fluid">
        </slide>
        <slide>
          <img src="../../assets/carousel-three.png" class="img-fluid">
        </slide>
      </carousel>
      <!-- Html markup end-->
    </div>
  </div>
</template>

<script>
  import carousel from '../../components/carousel'
  import slide from '../../components/slide'

  export default {
    components: {
      carousel,
      slide
    },
    // @prams interval为轮播时间 controls 为上下箭头 indicators 为下面nav
    data () {
      return {
        interval: 3000,
        controls: true,
        indicators: true
      }
    }
  }
</script>
