<template>
  <div class="bg">
    <div id="p1" ref="p1" class="page bg-gray-100">Page1</div>
    <div id="p2" ref="p2" class="page bg-gray-300">Page2</div>
    <div id="p3" ref="p3" class="page bg-gray-500">Page3</div>
    <div id="p4" ref="p4" class="page bg-gray-700">Page4</div>
    <div id="p5" ref="p5" class="page bg-gray-800">Page5</div>
  </div>
</template>

<script>
import VueScrollTo from 'vue-scrollto'

export default {
  name: "Scroll",
  data() {
    return {
      supportsWheel: false,
      // isScrolling: false,
      current: 1,
      maxPage: 5,
      throttleTime: 2000,
      throttleCounter: 0,

      //block scroll
      isDisableScroll: false,
      wheelOpt: null,
      wheelEvent: null,
      keys: {37: 1, 38: 1, 39: 1, 40: 1}



    }
  },
  created() {
    this.registerEvents()

    //block scroll init
    let supportsPassive = false
    try {
      window.addEventListener("test", null, Object.defineProperty({}, 'passive', {
        get: function () {
          supportsPassive = true
        }
      }))
    } catch (e) {
    }

    this.wheelOpt = supportsPassive ? {passive: false} : false
    this.wheelEvent = 'onwheel' in document.createElement('div') ? 'wheel' : 'mousewheel'
  },
  destroyed() {
    this.removeEvents()
  },
  methods: {
    registerEvents() {
      document.addEventListener('wheel', this.DoSomething)
      document.addEventListener('mousewheel', this.DoSomething)
      document.addEventListener('DOMMouseScroll', this.DoSomething)
    },
    removeEvents() {
      document.removeEventListener('wheel', this.DoSomething)
      document.removeEventListener('mousewheel', this.DoSomething)
      document.removeEventListener('DOMMouseScroll', this.DoSomething)
    },
    DoSomething(e) {
      if (this.throttleCounter > 0) {
        if(!this.isDisableScroll) {
          // this.disableScroll()
          this.isDisableScroll = true
        }
        return
      }
      console.log('DoSomething with Debounce')

      if (this.throttleCounter <= 0) {
        this.throttleCounter = this.throttleTime

        const id = setInterval(() => {
          this.throttleCounter -= 1000
          if (this.throttleCounter <= 0) {
            // this.enableScroll()
            this.isDisableScroll = false
            clearInterval(id)
          }
        }, 1000)
      }


      console.log('e', e)
      /* Check whether the wheel event is supported. */
      if (e.type === "wheel") this.supportsWheel = true
      else if (this.supportsWheel) return

      /* Determine the direction of the scroll (< 0 → up, > 0 → down). */
      const delta = ((e.deltaY || -e.wheelDelta || e.detail) >> 10) || 1

      // console.log(delta)
      this.current += delta

      // Handle index range problem
      if (this.current <= 0) this.current = this.maxPage
      if (this.current > this.maxPage) this.current = 1

      this.scrollTo(this.current)
    },

    scrollTo(targetPage) {
      const options = {
        easing: 'ease-in',
        offset: 0,
        force: true,
        cancelable: true,
        onStart: (element) => {
          // scrolling started
          console.log('onStart')
          this.disableScroll()
        },
        onDone: (element) => {
          // scrolling is done
          console.log('onDone')
          this.enableScroll()
        },
        onCancel: () => {
          // scrolling has been interrupted
        },
        x: false,
        y: true
      }

      console.log('targetPage', targetPage)
      const cancelScroll = VueScrollTo.scrollTo(`#p${targetPage}`, 500, options)
      console.log('VueScrollTo.scrollTo', `#p${targetPage}`)
    },

    /**
     * Block Scroll
     **/

    preventDefaultForScrollKeys(e) {
      if (this.keys[e.keyCode]) {
        e.preventDefault()
        return false
      }
    },

    preventDefault(e) {
      e.preventDefault();
    },

    disableScroll() {
      console.log('----disableScroll')
      window.addEventListener('DOMMouseScroll', this.preventDefault, false) // older FF
      window.addEventListener(this.wheelEvent, this.preventDefault, this.wheelOpt) // modern desktop
      window.addEventListener('touchmove', this.preventDefault, this.wheelOpt) // mobile
      window.addEventListener('keydown', this.preventDefaultForScrollKeys, false)
    },

    enableScroll() {
      console.log('++++enableScroll')
      window.removeEventListener('DOMMouseScroll', this.preventDefault, false)
      window.removeEventListener(this.wheelEvent, this.preventDefault, this.wheelOpt)
      window.removeEventListener('touchmove', this.preventDefault, this.wheelOpt)
      window.removeEventListener('keydown', this.preventDefaultForScrollKeys, false)
    }


  }
}
</script>

<style lang="stylus" scoped>
.page
  @apply h-screen flex justify-center items-center
</style>
