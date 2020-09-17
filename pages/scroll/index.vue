<template>
  <div class="bg">
    <div class="page bg-gray-100">Page1</div>
    <div class="page bg-gray-300">Page2</div>
    <div class="page bg-gray-500">Page3</div>
    <div class="page bg-gray-700">Page4</div>
    <div class="page bg-gray-800">Page5</div>
  </div>
</template>

<script>
export default {
  name: "Scroll",
  created() {
    window.addEventListener('scroll', this.handleScroll);
  },
  destroyed () {
    window.removeEventListener('scroll', this.handleScroll);
  },
  methods:{
    handleScroll(e){
      const doc = document.documentElement
      const top = (window.pageYOffset || doc.scrollTop) - (doc.clientTop || 0)
      console.log('onScroll',top)
    }
  }
}
</script>

<style lang="stylus" scoped>
.page
  @apply h-screen flex justify-center items-center
</style>
