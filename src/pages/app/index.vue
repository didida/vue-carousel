<template>
  <div class="">
    <div slot="markup">
      <!-- Html markup start-->
      <carousel
        :interval="5000",
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
    data () {
      return {
        controls: true,
        indicators: true
      }
    }
  }
</script>
