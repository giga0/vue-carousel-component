<template>
  <div id="app">
    <h1>Carousel example</h1>
    <div class="carousel-parent">
      <carousel :images="images"
        :gap="15"
        slideCursor="zoom-in"
        :goToSlide="input"
        :arrowIcon="arrow"
        :arrowsRotation="arrowsRotation"
        @imgClicked="imgClicked"
        @beforeSlideMove="slideMoveEvent = 'beforeSlideMove'"
        @afterSlideMove="slideMoveEvent = 'afterSlideMove'">
      </carousel>
    </div>
    <p><strong>Slide move event:</strong> {{ slideMoveEvent }}</p>
    <label for="input"><strong>Go to slide (from parent component, type: left, right or number of slide): </strong></label>
    <input id="input" type="text" v-model="input" />
    <ul v-if="img">
      <strong>Image clicked</strong>
      <li><span>src</span>: <i>{{ img.src }}</i></li>
      <li><span>alt</span>: <i>{{ img.alt }}</i></li>
    </ul>
  </div>
</template>

<script>
import Carousel from '@/components/Carousel'
import Arrow from '@/components/Arrow'

export default {
  name: 'app',
  components: { Carousel },
  data () {
    return {
      input: '',
      img: null,
      slideMoveEvent: 'nothing for now...',
      images: [
        { src: require('@/assets/nature-benefits.jpg.860x0_q70_crop-scale.jpg'), alt: 'nature-1'},
        { src: require('@/assets/nature_slovenia_s.jpg'), alt: 'nature-2'},
        { src: require('@/assets/02_01_slide_nature.jpg'), alt: 'nature-3'},
        { src: require('@/assets/naturewildlife-1000x1000.jpg'), alt: 'nature-4'},
        { src: require('@/assets/88-tendon-cascade-de-tendon-gilles-pecqueur-600x600.jpg'), alt: 'nature-5'}
      ],
      arrow: Arrow,
      arrowsRotation: { leftArrow: '360deg', rightArrow: '180deg' }
    }
  },
  methods: {
    imgClicked(image) {
      this.img = image
    }
  }
}
</script>

<style lang="scss" rel="stylesheet/scss">
body {
  background-color: antiquewhite;
  margin: 0;
}

#app {
  font-family: 'Courier New', Courier, monospace;
  margin: 30px;

  @media screen and (max-width: 767px) {
    margin: 15px;
  }

  h1 {
    font-size: 22px;
    margin: 20px 0;

    @media screen and (max-width: 767px) {
      font-size: 20px;
    }
  }

  p {
    font-size: 12px;
    margin: 20px 0;

    @media screen and (max-width: 767px) {
      font-size: 10px;
    }
  }

  label {
    font-size: 12px;

    @media screen and (max-width: 767px) {
      font-size: 10px;
    }
  }

  input {
    font-size: 12px;
    margin-bottom: 20px;

    @media screen and (max-width: 767px) {
      font-size: 10px;
    }
  }

  ul {
    list-style: none;
    font-size: 12px;
    width: 60%;
    margin: 10px 0;
    padding: 0;

    @media screen and (max-width: 767px) {
      font-size: 10px;
      width: 90%;
    }
  }

  li {
    margin-top: 5px;

    span {
      text-decoration: underline;
    }
  }

  .carousel-parent {
    width: 60%;
    height: 400px;
    padding: 20px;
    background-color: floralwhite;

    @media screen and (max-width: 767px) {
      width: 90%;
      height: 200px;
    }
  }
}
</style>
