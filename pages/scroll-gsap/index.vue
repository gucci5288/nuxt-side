<template>
  <div class="all-wrap">
    <div class="container">

      <div class="page-wrap">
        <div id="page-trigger-1" class="central absolute">
          <div id="page-content-1">My name is Gucci</div>
        </div>
        <div id="bg-1" class="bg-settings" />
      </div>

      <div class="page-wrap">
        <div id="page-trigger-2" class="central absolute ">
          <div id="page-content-2">A Frontend Web Developer</div>
        </div>
        <div id="bg-2" class="bg-settings" />
      </div>

      <div class="page-wrap">
        <div id="page-trigger-3" class="central absolute ">
          <div id="page-content-3">JavaScript, VueJS, Vuex, RxJs</div>
        </div>
        <div id="bg-3" class="bg-settings" />
      </div>

      <div class="page-wrap">
        <div id="page-trigger-4" class="central absolute ">
          <div id="page-content-4">Webpack, ReactJS, Redux, GraphQL</div>
        </div>
        <div id="bg-4" class="bg-settings" />
      </div>

      <div class="page-wrap">
        <div id="page-trigger-5" class="central absolute ">
          <div id="page-content-5">Stay hungry, Stay foolish.</div>
        </div>
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


    // const TL = new gsap.timeline().
    // console.log('TL',TL)

    // new TimelineMax.staggerFrom(chars, 0.8, {
    //   opacity: 0,
    //   scale: 0,
    //   y: 80,
    //   rotationX: 180,
    //   transformOrigin: "0% 50% -50",
    //   ease: Back.easeOut
    // }, 0.01)


    gsap.registerPlugin(ScrollTrigger)
    this.initParallaxBgs()


    /**
     * page 1 text
     */
    const splitText1 = new SplitText("#page-content-1", {type: "chars"}) //{type:"chars, words, lines"}
    splitText1.chars.forEach((char, index) => {
      this.initGsapFlyInScrollStart(char, '#page-trigger-1', index + 1)
    })

    /**
     * page 2 text
     */
    const splitText2 = new SplitText("#page-content-2", {type: "words"}) //{type:"chars, words, lines"}
    splitText2.words.forEach((word, index) => {
      this.initGsapFlyInScroll(word, '#page-trigger-2', index + 1, 'word')
    })

    /**
     * page 3 text
     */
    const splitText3 = new SplitText("#page-content-3", {type: "words"}) //{type:"chars, words, lines"}
    splitText3.words.forEach((word, index) => {
      this.initGsapFlyInScroll(word, '#page-trigger-3', index + 1, 'word')
    })

    /**
     * page 4 text
     */
    const splitText4 = new SplitText("#page-content-4", {type: "words"}) //{type:"chars, words, lines"}
    splitText4.words.forEach((word, index) => {
      this.initGsapFlyInScroll(word, '#page-trigger-4', index + 1, 'word')
    })

    /**
     * page 5 text
     */
    const splitText5 = new SplitText("#page-content-5", {type: "words"}) //{type:"chars, words, lines"}
    splitText5.words.forEach((word, index) => {
      this.initGsapFlyInScroll(word, '#page-trigger-5', index + 1, 'word', true)
    })

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
        x: '150vw',
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

        start = `top ${60 - delay * multiple}%`
        end = `top ${60 - delay * multiple}%`
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
        x: '150vw',
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
          trigger: ".container",
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

// 字
.central
  z-index 20
  width 70vw
  transform translate(-100vw, 50vh);

  div
    color white
    font-size 2rem
    position: absolute;
    text-align center

  //width 50vw
  //left: 50%;

  div:nth-child(1)
    transform: translate(-50%, -70%);

  div:nth-child(2)
    transform: translate(-50%, 0%);

  div:nth-child(3)
    transform: translate(-50%, 70%);

.bg-settings
  @apply h-screen bg-cover bg-center bg-no-repeat absolute bg-fixed
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;

#bg-1
  filter: blur(2px);
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
