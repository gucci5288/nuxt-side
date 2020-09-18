<template>
  <div class="container">
    <div id="p1" class="page bg-gray-100">Page1</div>
    <div id="p2" class="page bg-gray-300">Page2</div>
    <div id="p3" class="page bg-gray-500">Page3</div>
    <div id="p4" class="page bg-gray-700">Page4</div>
    <div id="p5" class="page bg-gray-800">Page5</div>
    <div id="p6" class="page bg-gray-900">Page6</div>
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

    let sections = gsap.utils.toArray(".page")
    let container = gsap.utils.toArray('.container')

    console.log('containerGsap', container[0])
    console.log(container[0].offsetHeight)

    console.log('sections', sections)

    gsap.to(sections, {
      yPercent: -100 * (sections.length - 1),
      ease: "none",
      scrollTrigger: {
        trigger: ".container",
        pin: true, // pin the trigger element while active
        scrub: 0.5, // smooth scrubbing, takes 1 second to "catch up" to the scrollbar
        snap: 1 / (sections.length - 1),
        // base vertical scrolling on how wide the container is so it feels more natural.
        end: () => "+=" + container[0].offsetHeight
      }
    })

  }
}
</script>

<style lang="stylus" scoped>
.container
  height 500vh

.page
  @apply h-screen flex justify-center items-center
</style>
