<template>
  <div class="drogue" :class="{ 'close': isClose }">
    <slot></slot>
  </div>
</template>
<script>
export default {
  props: {
    delay: {
      type: Number,
      default: 1500
    }
  },
  data() {
    return {
      isClose: false,
      timer: null
    }
  },
  mounted() {
    this.initScroll()
  },
  destroyed() {
    this.destroyScroll()
  },
  methods: {
    initScroll() {
      document.addEventListener('scroll', ev => {
        this.isClose = true
        this.destroyScroll()
        this.timer = setTimeout(() => {
          this.isClose = false
        }, this.delay)
      })
    },
    destroyScroll() {
      clearTimeout(this.timer)
      this.timer = null
    }
  }
}
</script>
<style scoped>
.drogue {
  position: fixed;
  right: 0;
  bottom: 50px;
  padding: 20px 10px;
  -webkit-transition: transform 0.3s;
  transition: transform 0.3s;
}

.close {
  -webkit-transform: translateX(650%);
  transform: translateX(65%);
}
</style>