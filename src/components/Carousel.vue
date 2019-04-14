<template>
  <div class="carousel">
    <!-- Carousel wrapper -->
    <div :style="{ height: imgHeight }"
      class="carousel-wrapper"
      @mouseenter="mouseEnterWrapper"
      @mouseleave="mouseLeaveWrapper">
      <!-- Carousel track -->
      <div v-if="images && images.length > 0"
        :style="{ left: trackLeft + 'px' }"
        class="carousel-track">
        <!-- Carousel slides -->
        <div v-for="(image, index) in images"
          :key="`carousel-slide_${index}`"
          :style="{ 'margin-right': gap + 'px', cursor: slideCursor }"
          :id="`carousel-slide_${index}`"
          class="carousel-slide">
          <img class="carousel-slide-image"
            :src="image.src"
            :alt="image.alt"
            @load="slidesPositionCheck"
            @click="imgClick(image)" />
        </div>
      </div>
      <!-- Carousel note if there is no images/slides -->
      <div v-else
        class="carousel-note">
        {{ noImgNote }}
      </div>
      <!-- Carousel left arrow -->
      <div v-if="arrows && images && images.length > 0"
        :style="{ left: arrowsGap.left }"
        class="carousel-arrow carousel-arrow--left"
        @click="goTo('right')">
        <component v-if="arrowIcon"
          :is="arrowIcon"
          :style="{ 'background-color': arrowsColors['background-color'], transform: `rotate(${arrowsRotation.leftArrow})` }"
          :width="arrowsDimension.width"
          :height="arrowsDimension.height"
          :color="arrowsColors.color">
        </component>
        <svg v-if="!arrowIcon"
          :style="{ 'background-color': arrowsColors['background-color'], transform: 'rotate(180deg)' }"
          :width="arrowsDimension.width"
          :height="arrowsDimension.height"
          xmlns="http://www.w3.org/2000/svg"
          xmlns:xlink="http://www.w3.org/1999/xlink"
          viewBox="0 0 24 24">
          <g :fill="arrowsColors.color">
            <path d="M9.295 17.045a.998.998 0 0 1 0-1.412l3.875-3.883-3.874-3.883a.997.997 0 0 1-.001-1.412 1.005 1.005 0 0 1 1.417.007l4.576 4.576a1.01 1.01 0 0 1 0 1.424l-4.576 4.576a1 1 0 0 1-1.417.007z"/>
          </g>
        </svg>
      </div>
      <!-- Carousel right arrow -->
      <div v-if="arrows && images && images.length > 0"
        :style="{ right: arrowsGap.right }"
        class="carousel-arrow carousel-arrow--right"
        @click="goTo('left')">
        <component v-if="arrowIcon"
          :is="arrowIcon"
          :style="{ 'background-color': arrowsColors['background-color'], transform: `rotate(${arrowsRotation.rightArrow})` }"
          :width="arrowsDimension.width"
          :height="arrowsDimension.height"
          :color="arrowsColors.color">
        </component>
        <svg v-if="!arrowIcon"
          :style="{ 'background-color': arrowsColors['background-color'] }"
          :width="arrowsDimension.width"
          :height="arrowsDimension.height"
          xmlns="http://www.w3.org/2000/svg"
          xmlns:xlink="http://www.w3.org/1999/xlink"
          viewBox="0 0 24 24">
          <g :fill="arrowsColors.color">
            <path d="M9.295 17.045a.998.998 0 0 1 0-1.412l3.875-3.883-3.874-3.883a.997.997 0 0 1-.001-1.412 1.005 1.005 0 0 1 1.417.007l4.576 4.576a1.01 1.01 0 0 1 0 1.424l-4.576 4.576a1 1 0 0 1-1.417.007z"/>
          </g>
        </svg>
      </div>
    </div>
    <!-- Carousel dots -->
    <div v-if="dots && images && images.length > 0"
      :style="{ 'margin-top': dotsMarginTop, 'justify-content': dotsJustify }"
      class="carousel-dots">
      <span v-for="(image, index) in images"
        :key="`carousel-dot_${index}`"
        :id="`carousel-dot_${index}`"
        :style="{
          width: dotsDimension.width,
          height: dotsDimension.height,
          'margin-right': dotsGap
        }"
        class="dot"
        @click="goTo(index)">
      </span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Carousel',
  props: {
    images: {
      type: Array,
      validator: val => {
        if (val.length > 0) {
          for (let el of val) {
            if (!el.src || typeof el.src !== 'string') el.src = ''
            if (!el.alt || typeof el.alt !== 'string') el.alt = ''
          }
        }
        return true
      }
    },
    goToSlide: [ String, Number ],
    imgHeight: { type: String, default: '88%' },
    slideCursor: { type: String, default: 'default' },
    gap: { type: Number, default: 10 },
    arrows: { type: Boolean, default: true },
    arrowIcon: { type: Object },
    dots: { type: Boolean, default: true },
    arrowsColors: {
      type: Object,
      validator: val => {
        for (let el in val) {
          // check if given dimensions are string
          if (typeof val[el] === 'string') return true
          // if not, give them default value
          if (el === 'background-color') val[el] = '#0080FF'
          if (el === 'color') val[el] = '#fff'
        }
        return true
      },
      default: () => {
        return {
          'background-color': '#0080FF',
          color: '#fff'
        }
      }
    },
    arrowsDimension: {
      type: Object,
      validator: val => {
        for (let el in val) {
          // check if given dimensions are string
          if (typeof val[el] === 'string') return true
          // if not, give them default value
          val[el] = '30px'
        }
        return true
      },
      default: () => {
        return {
          width: '30px',
          height: '30px'
        }
      }
    },
    arrowsRotation: {
      type: Object,
      validator: val => {
        for (let el in val) {
          // check if given dimensions are string
          if (typeof val[el] === 'string') return true
          // if not, give them default value
          val[el] = '0deg'
        }
        return true
      },
      default: () => {
        return {
          leftArrow: '0deg',
          rightArrow: '0deg'
        }
      }
    },
    arrowsGap: {
      type: Object,
      validator: val => {
        for (let el in val) {
          // check if given gaps are string
          if (typeof val[el] === 'string') return true
          // if not, give them default value
          val[el] = '5px'
        }
        return true
      },
      default: () => {
        return {
          left: '5px',
          right: '5px'
        }
      }
    },
    dotsMarginTop: { type: String, default: '18px' },
    dotsJustify: { type: String, default: 'flex-start' },
    dotsDimension: {
      type: Object,
      validator: val => {
        for (let el in val) {
          // check if given dimensions are string
          if (typeof val[el] === 'string') return true
          // if not, give them default value
          val[el] = '9px'
        }
        return true
      },
      default: () => {
        return {
          width: '9px',
          height: '9px'
        }
      }
    },
    dotsGap: { type: String, default: '9px' },
    noImgNote: { type: String, default: 'No image(s) to show.' }
  },
  data () {
    return {
      trackLeft: 0,
      sliding: false
    }
  },
  watch: {
    goToSlide (val) {
      if (val === 'left' || val === 'right') {
        this.goTo(val)
      } else {
        val = parseInt(val)
        if (Number.isInteger(val) && val > 0 && val <= this.images.length) {
          this.goTo(val - 1)
        } else false
      }
    }
  },
  methods: {
    imgClick (image) {
      this.$emit('imgClicked', image)
    },
    // calculations needed often, so they are provided by this method that returns an object
    offsetCalculations () {
      const vm = this
      const wrapperViewport = document.getElementsByClassName('carousel-wrapper')[0]
      const wrapperViewportOffset = wrapperViewport.getBoundingClientRect()

      return {
        wrapperViewportWidth: Math.round(wrapperViewport.offsetWidth),
        wrapperViewportOffsetLeft: Math.round(wrapperViewportOffset.left),
        wrapperViewportOffsetRight: Math.round(wrapperViewportOffset.right),
        firstSlideOffsetLeft: Math.round(document.getElementById('carousel-slide_0').getBoundingClientRect().left),
        lastSlideOffsetRight: Math.round(document.getElementById(`carousel-slide_${vm.images.length - 1}`).getBoundingClientRect().right)
      }
    },
    mouseEnterWrapper () {
      const contentWidth = window.innerWidth

      if (contentWidth > 767 && this.arrows && this.images && this.images.length > 0) {
        const leftArrow = document.getElementsByClassName('carousel-arrow--left')[0]
        const rightArrow = document.getElementsByClassName('carousel-arrow--right')[0]
        leftArrow.classList.add('active')
        rightArrow.classList.add('active')
      }
    },
    mouseLeaveWrapper () {
      const contentWidth = window.innerWidth

      if (contentWidth > 767 && this.arrows && this.images && this.images.length > 0) {
        const leftArrow = document.getElementsByClassName('carousel-arrow--left')[0]
        const rightArrow = document.getElementsByClassName('carousel-arrow--right')[0]
        leftArrow.classList.remove('active')
        rightArrow.classList.remove('active')
      }
    },
    // core slider move method
    goTo (event) {
      // current slides in view
      const slidesInView = document.getElementsByClassName('carousel-slide in-view')
      for (let i = 0; i < slidesInView.length; i++) {
        // check if any of their id's are equal to event
        // if it is -> escape from method
        if (event === parseInt(slidesInView[i].id.split('_').pop())) {
          return
        }
      }

      if (!this.sliding) {
        this.sliding = true
        this.$emit('beforeSlideMove')
        const calc = this.offsetCalculations()

        if (event === 'left') {
          // case for left arrow & left keyCode
          // when going on left we are interested in width od first slide 'in-view'
          const currentSlideWidth = Math.round(document.getElementsByClassName('carousel-slide in-view')[0].offsetWidth)
          const leftArrow = document.getElementsByClassName('carousel-arrow--left')[0]
          // on left keyCode short time left arrow display for better UX
          if (window.getComputedStyle(leftArrow).visibility === 'hidden') {
            leftArrow.classList.add('active')
            setTimeout(() => {
              leftArrow.classList.remove('active')
            }, 275)
          }
          // slider move to left (two diff cases here)
          if (calc.wrapperViewportOffsetRight <= calc.lastSlideOffsetRight) {
            this.trackLeft = this.trackLeft - currentSlideWidth - this.gap
          } else this.trackLeft = 0
          /////////////////////////////////////
        } else if (event === 'right') {
          // case for right arrow & right keyCode
          // when going on right we are interested in width od first slide before one that is 'in-view'
          let beforeCurrentSlideWidth
          if (document.getElementsByClassName('carousel-slide in-view')[0].id === 'carousel-slide_0') {
            const i = this.images.length - 1
            beforeCurrentSlideWidth = document.getElementById(`carousel-slide_${i}`).offsetWidth
          } else beforeCurrentSlideWidth = Math.round(document.getElementsByClassName('carousel-slide in-view')[0].previousSibling.offsetWidth)
          const rightArrow = document.getElementsByClassName('carousel-arrow--right')[0]
          // on right keyCode short time right arrow display for better UX
          if (window.getComputedStyle(rightArrow).visibility === 'hidden') {
            rightArrow.classList.add('active')
            setTimeout(() => {
              rightArrow.classList.remove('active')
            }, 275)
          }
          // slider move to right (two diff cases here)
          if (calc.wrapperViewportOffsetLeft > calc.firstSlideOffsetLeft) {
            this.trackLeft = this.trackLeft + beforeCurrentSlideWidth + this.gap
          } else {
            // this case is a bit complex as it's not goal to jump blindly on last slide
            // but to potentially show slide/s before last one if it's possible (depending on a viewport)
            const imagesLength = this.images.length
            const lastSlide = document.getElementById(`carousel-slide_${imagesLength - 1}`)
            const lastSlideWidth = Math.round(lastSlide.offsetWidth)
            const lastSlideOffsetLeft = Math.round(lastSlide.getBoundingClientRect().left)

            if (lastSlideWidth + this.gap >= calc.wrapperViewportWidth) {
              this.trackLeft = this.trackLeft + calc.wrapperViewportOffsetLeft - lastSlideOffsetLeft
            } else {
              let slidesToDisplayWidth = lastSlideWidth + this.gap
              for (let j = imagesLength - 2; j >= 0; j--) {
                const prevSlide = document.getElementById(`carousel-slide_${j}`)
                const prevSlideWidth = Math.round(prevSlide.offsetWidth)
                const prevSlideOffsetLeft = Math.round(prevSlide.getBoundingClientRect().left)
                slidesToDisplayWidth = slidesToDisplayWidth + prevSlideWidth + this.gap
                if (slidesToDisplayWidth >= calc.wrapperViewportWidth) {
                  slidesToDisplayWidth = slidesToDisplayWidth - prevSlideWidth - this.gap
                  this.trackLeft = this.trackLeft + calc.wrapperViewportOffsetLeft - prevSlideOffsetLeft - prevSlideWidth - this.gap
                  break
                }
              }
            }
          }
          /////////////////////////////////////
        } else if (Number.isInteger(event)) {
          // case for click on the dots
          const i = event
          if (i === 0) {
            this.trackLeft = 0
          } else {
            // this case is a bit complex as it's not goal to jump blindly on any slide
            // but to also show slide/s before or after that one if it's possible (depending on a viewport)
            const slide = document.getElementById(`carousel-slide_${i}`)
            const slideWidth = Math.round(slide.offsetWidth)
            const slideOffsetLeft = Math.round(slide.getBoundingClientRect().left)

            if (slideWidth + this.gap >= calc.wrapperViewportWidth) {
              this.trackLeft = this.trackLeft + calc.wrapperViewportOffsetLeft - slideOffsetLeft
            } else {
              let slidesToDisplayWidth = slideWidth + this.gap
              for (let j = i + 1; j < this.images.length; j++) {
                const nextSlideWidth = Math.round(document.getElementById(`carousel-slide_${j}`).offsetWidth)
                slidesToDisplayWidth = slidesToDisplayWidth + nextSlideWidth + this.gap
                if (slidesToDisplayWidth >= calc.wrapperViewportWidth) break
              }
              if (slidesToDisplayWidth >= calc.wrapperViewportWidth) {
                this.trackLeft = this.trackLeft + calc.wrapperViewportOffsetLeft - slideOffsetLeft
              } else {
                for (let j = i - 1; j >= 0; j--) {
                  const prevSlide = document.getElementById(`carousel-slide_${j}`)
                  const prevSlideWidth = Math.round(prevSlide.offsetWidth)
                  const prevSlideOffsetLeft = Math.round(prevSlide.getBoundingClientRect().left)
                  slidesToDisplayWidth = slidesToDisplayWidth + prevSlideWidth + this.gap
                  if (slidesToDisplayWidth >= calc.wrapperViewportWidth) {
                    slidesToDisplayWidth = slidesToDisplayWidth - prevSlideWidth - this.gap
                    this.trackLeft = this.trackLeft + calc.wrapperViewportOffsetLeft - prevSlideOffsetLeft - prevSlideWidth - this.gap
                    break
                  }
                }
              }
            }
          }
          /////////////////////////////////////
        } else this.sliding = false
        // setTimeout is needed because of carousel track transition
        setTimeout(() => {
          this.slidesPositionCheck()
          this.sliding = false
          this.$emit('afterSlideMove')
        }, 300)
      }
    },
    slidesPositionCheck () {
      const calc = this.offsetCalculations()
      const carouselTrack = document.getElementsByClassName('carousel-track')[0]
      // loop through slides for their offsets and needed checks
      for (let i = 0; i < carouselTrack.children.length; i++) {
        let slide = carouselTrack.children[i]
        let slideWidth = carouselTrack.children[i].offsetWidth
        let slideLeft = Math.round(slide.getBoundingClientRect().left)
        let slideRight = Math.round(slide.getBoundingClientRect().right)
        let slideDot = document.getElementById(`carousel-dot_${i}`)

        // had to make small hack (+ 5px) because js doesn't calculate precisely
        // guess the reason is width relative to fixed height
        if (slideLeft + 5 >= calc.wrapperViewportOffsetLeft &&
        slideRight <= calc.wrapperViewportOffsetRight) {
          slide.classList.add('in-view')
          slide.classList.remove('part-in-view')

          if (this.dots && this.images && this.images.length > 0) {
            slideDot.classList.add('in-view')
            slideDot.classList.remove('part-in-view')
          }
        } else if (slideLeft + 5 >= calc.wrapperViewportOffsetLeft &&
        slideLeft < calc.wrapperViewportOffsetRight &&
        slideRight > calc.wrapperViewportOffsetRight) {
          if (slideWidth >= calc.wrapperViewportWidth &&
          slideLeft + 5 - calc.wrapperViewportOffsetLeft < 6) {
            slide.classList.add('in-view')
            slide.classList.remove('part-in-view')

            if (this.dots && this.images && this.images.length > 0) {
              slideDot.classList.add('in-view')
              slideDot.classList.remove('part-in-view')
            }
          } else {
            slide.classList.add('part-in-view')
            slide.classList.remove('in-view')

            if (this.dots && this.images && this.images.length > 0) {
              slideDot.classList.add('part-in-view')
              slideDot.classList.remove('in-view')
            }
          }
        } else {
          slide.classList.remove('in-view', 'part-in-view')

          if (this.dots && this.images && this.images.length > 0) {
            slideDot.classList.remove('in-view', 'part-in-view')
          }
        }
      }
    }
  },
  mounted () {
    document.onkeydown = (e) => {
      if (this.images && this.images.length > 0) {
        e = e || window.event
        switch (e.which || e.keyCode) {
          case 37: this.goTo('right')
            break
          case 39: this.goTo('left')
            break
          default: return
        }
        e.preventDefault()
      }
    }
  }
}
</script>

<style lang="scss" rel="stylesheet/scss" scoped>
.carousel {
  width: 100%;
  height: 100%;

  &-wrapper {
    position: relative;
    overflow: hidden;
  }

  &-track {
    position: relative;
    display: flex;
    height: 100%;
    transition: all .25s ease;
  }

  &-slide {
    display: inline-block;
    height: 100%;
    opacity: .5;
    box-shadow: 0 8px 16px 0 rgba(0, 0, 0, 0.05);
    transition: all .25s ease;

    &.in-view {
      opacity: 1;
    }

    &.part-in-view {
      pointer-events: none;
    }

    &-image {
      height: 100%;
    }
  }

  &-note {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }

  &-arrow {
    z-index: 1;
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    display: flex;
    visibility: hidden;
    opacity: 0;
    transition: all .25s ease;

    @media screen and (max-width: 767px) {
      visibility: visible;
      opacity: .95;
    }

    &.active {
      visibility: visible;
      opacity: .95;
    }

    svg {
      opacity: 1;
      border-radius: 50%;
      transition: all .25s ease;
      cursor: pointer;
    }
  }

  &-dots {
    display: flex;

    .dot {
      background-color: #e5e5e5;
      border-radius: 50%;
      transition: all .25s ease;
      cursor: pointer;

      &.in-view {
        background-color: #626262;
      }

      &.part-in-view {
        background-color: #9b9b9b;
      }
    }
  }
}
</style>
