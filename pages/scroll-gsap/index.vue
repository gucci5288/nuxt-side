<template>
  <div class="all-wrap">
    <div class="container">

      <div class="page-wrap">
        <div id="name" class="central absolute">
          <div>My name is Gucci</div>
        </div>
        <div id="bg-1" class="bg-settings h-screen bg-cover bg-center" />
      </div>

      <div class="page-wrap">
        <div id="job-title" class="central absolute ">
          <div>A Frontend Web Developer</div>
        </div>
        <div id="bg-2" class="bg-settings h-screen bg-cover bg-center" />
      </div>

      <div class="page-wrap">
        <div id="job-title2" class="central absolute ">
          <div>JavaScript</div>
          <div>VueJS / Vuex</div>
          <div>RxJS</div>
        </div>
        <div id="bg-3" class="bg-settings h-screen bg-cover bg-center" />
      </div>

      <div class="page-wrap">
        <div id="job-title3" class="central absolute ">
          <div>Webpack</div>
          <div>ReactJS / Redux</div>
          <div>GraphQL</div>
        </div>
        <div id="bg-4" class="bg-settings h-screen bg-cover bg-center" />
      </div>

      <div class="page-wrap">
        <div id="job-title4" class="central absolute ">
          <div>Stay hungry, Stay foolish.</div>
        </div>
        <div id="bg-5" class="bg-settings h-screen bg-cover bg-center" />
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
  created() {

  },
  mounted() {

    gsap.registerPlugin(ScrollTrigger)
    this.initParallaxBgs()
    this.initGsapFlyInScrollStart('#name')
    this.initGsapFlyInScroll('#job-title')
    this.initGsapFlyInScroll('#job-title2')
    this.initGsapFlyInScroll('#job-title3')
    this.initGsapFlyInScroll('#job-title4')
    // this.initGsap()
  },
  methods: {
    //toggleActions
    //onEnter, onLeave, onEnterBack, and onLeaveBack,
    //"play", "pause", "resume", "reset", "restart", "complete", "reverse", and "none".
    initGsapFlyInScrollStart(target) {
      gsap.to(target, {
        scrollTrigger: {
          trigger: target,
          start: 'top 49.9%',
          end: 'top 49.9%',
          scrub: 1,
          // markers: true,

          toggleActions: 'restart pause reverse pause'
        },
        x: '25vw',
        duration: 1
      })
    },
    initGsapFlyInScroll(target) {
      gsap.to(target, {
        scrollTrigger: {
          trigger: target,
          start: 'top 60%',
          end: 'top 60%',
          scrub: 1,
          // markers: true,
          toggleActions: 'restart pause reverse pause'
        },
        x: '25vw',
        duration: 1
      })
    },

    initParallaxBgs(){
      const bgs = gsap.utils.toArray(".bg-settings")
      gsap.utils.toArray(".page-wrap .bg-settings").forEach((page, i) => {
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
  width 50vw
  transform translate(-100vw, 50vh);

  div
    color white
    font-size 2rem
    position: absolute;
    text-align center
    width 50vw
    left: 50%;
  div:nth-child(1)
    transform: translate(-50%, -70%);
  div:nth-child(2)
    transform: translate(-50%, 0%);
  div:nth-child(3)
    transform: translate(-50%, 70%);

.bg-settings
  background-attachment fixed
  background-repeat no-repeat
  position: absolute;
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

//height 100vh

//.flex-sets
//  @apply flex justify-center items-center
</style>
