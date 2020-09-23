<template>
  <div class="all-wrap">
    <div class="container-control">

      <div class="page-wrap">
        <div id="page-content-1" class="central">My name is Gucci</div>
        <div id="bg-1" class="bg-settings" />
      </div>

      <div class="page-wrap">
        <div id="page-content-2" class="central">A Frontend Web Developer</div>
        <div id="bg-2" class="bg-settings" />
      </div>

      <div class="page-wrap">
        <div id="page-content-3" class="central">JavaScript, VueJS, Vuex, RxJs</div>
        <div id="bg-3" class="bg-settings" />
      </div>

      <div class="page-wrap">
        <div id="page-content-4" class="central">Webpack, ReactJS, Redux, GraphQL</div>
        <div id="bg-4" class="bg-settings" />
      </div>

      <div class="page-wrap">
        <div id="page-content-5" class="central">Stay hungry, Stay foolish.</div>
        <div id="bg-5" class="bg-settings" />
      </div>
      <!--      <div class="page-wrap relative h-screen bg-gray-200">none</div>-->
    </div>
  </div>
</template>
<script>
import {gsap} from 'gsap'
import {ScrollTrigger} from "gsap/ScrollTrigger"

export default {
  name: "scroll-gsap",
  data() {
    return {
      name: 'My name is Gucci',
    }
  },
  created() {

  },
  mounted() {
    /**
     * to init parallax backgrounds
     */
    gsap.registerPlugin(ScrollTrigger)
    this.initParallaxBgs()
    /**
     * to init page 1 content
     */
    const splitText1 = new SplitText("#page-content-1", {type: "chars"}) //{type:"chars, words, lines"}
    splitText1.chars.forEach((char, index) => {
      this.initGsapFlyInScrollStart(char, '#page-content-1', index + 1)
    })
    /**
     * to init page 2 content
     */
    const splitText2 = new SplitText("#page-content-2", {type: "words"}) //{type:"chars, words, lines"}
    splitText2.words.forEach((word, index) => {
      this.initGsapFlyInScroll(word, '#page-content-2', index + 1, 'word')
    })
    /**
     * to init page 3 content
     */
    const splitText3 = new SplitText("#page-content-3", {type: "words"}) //{type:"chars, words, lines"}
    splitText3.words.forEach((word, index) => {
      this.initGsapFlyInScroll(word, '#page-content-3', index + 1, 'word')
    })
    /**
     * to init page 4 content
     */
    const splitText4 = new SplitText("#page-content-4", {type: "words"}) //{type:"chars, words, lines"}
    splitText4.words.forEach((word, index) => {
      this.initGsapFlyInScroll(word, '#page-content-4', index + 1, 'word')
    })

    /**
     * to init page 5 content
     */
    const splitText5 = new SplitText("#page-content-5", {type: "words"}) //{type:"chars, words, lines"}
    splitText5.words.forEach((word, index) => {
      this.initGsapFlyInScroll(word, '#page-content-5', index + 1, 'word', true)
    })

    // /**
    //  * init scrolling-page
    //  */
    // this.initGsap()
  },
  methods: {
    //toggleActions
    //onEnter, onLeave, onEnterBack, and onLeaveBack,
    //"play", "pause", "resume", "reset", "restart", "complete", "reverse", and "none".
    initGsapFlyInScrollStart(target, trigger, delay = 0) {
      gsap.to(target, {
        scrollTrigger: {
          trigger: trigger,
          start: `top ${49 - delay}%`,
          end: `top ${49 - delay}%`,
          scrub: 1,
          // markers: true,
          toggleActions: 'restart pause reverse pause'
        },
        x: '100vw',
        duration: 1,
      })
    },

    initGsapFlyInScroll(target, trigger, delay = 0, type = 'char', isLast = false) {

      let start = 'top 60%'
      let end = 'top 60%'

      if (type === 'char') {
        start = `top ${60 - delay}%`
        end = `top ${60 - delay}%`
      }

      if (type === 'word') {

        let multiple = 4
        if (isLast) multiple = 2

        start = `top ${70 - delay * multiple}%`
        end = `top ${70 - delay * multiple}%`
      }

      gsap.to(target, {
        scrollTrigger: {
          trigger: trigger,
          start,
          end,
          scrub: 1,
          // markers: true,
          toggleActions: 'restart pause reverse pause'
        },
        x: '100vw',
        duration: 1,
        delay
      })
    },

    initParallaxBgs() {
      const bgs = gsap.utils.toArray(".bg-settings")
      gsap.utils.toArray(".page-wrap").forEach((page, i) => {
        // Do the parallax effect on each section
        bgs[i].backgroundPosition = `50% ${innerHeight / 2}px`
        gsap.to(bgs[i], {
          backgroundPosition: `50% ${-innerHeight / 2}px`,
          ease: "none",
          scrollTrigger: {
            trigger: page,
            scrub: true,
            // markers: true,
          }
        })
      })
    },

    initGsap() {
      let sections = gsap.utils.toArray(".page")

      const endHeightPx = sections.reduce((prev, section) => {
        return prev + section.offsetHeight
      }, 0)

      console.log('endHeightPx', endHeightPx)

      gsap.to(sections, {
        yPercent: -100 * (sections.length - 1),
        ease: "none",
        scrollTrigger: {
          // markers: true,
          trigger: ".container-control",
          pin: true, // pin the trigger element while active
          scrub: 0.7, // smooth scrubbing, takes 1 second to "catch up" to the scrollbar
          snap: 1 / (sections.length - 1),
          // base vertical scrolling on how wide the container is so it feels more natural.
          end: () => "+=" + (endHeightPx - 667)
        }
      })
    }
  }
}
</script>

<style lang="stylus" scoped>
.page-wrap
  @apply relative h-screen w-screen
  background-color black

// 字 TODO Font-size
.central
  @apply relative text-3xl text-gray-100 z-20 text-center
  transform translate(-100%, 50vh);

.bg-settings
  @apply h-screen bg-cover bg-center bg-no-repeat absolute bg-fixed
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  filter: blur(2px);

#bg-1
  background-image: url(/code-bg-1.jpg)

#bg-2
  background-image: url(/code-bg-2.png)

#bg-3
  background-image: url(/code-bg-3.png)

#bg-4
  background-image: url(/code-bg-4.png)

#bg-5
  background-image: url(/code-bg-5.png)

.all-wrap
  overflow-x hidden
</style>
