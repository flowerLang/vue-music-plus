<template>
  <div ref="wrapper">
    <slot></slot>
  </div>
</template>

<script>
  import BScroll from 'better-scroll'

  export default {
    name: 'scroll',
    props: {
      click: {
        type: Boolean,
        default: true
      },
      data: {
        type: Array,
        default: null
      },
      probeType: {
        type: Number,
        default: 1
      },
      listenScroll: {
        type: Boolean,
        default: false
      },
      bounce: {
        type: Boolean,
        default: true
      },
      pullUp: {
        type: Boolean,
        default: false
      }
    },
    mounted () {
      setTimeout(() => {
        this._initScroll()
      }, 20)
    },
    watch: {
      data () {
        setTimeout(() => {
          this.refresh()
        }, 20)
      }
    },
    methods: {
      _initScroll () { // 初始化组件
        if (!this.$refs.wrapper) {
          return
        }
        this.scroll = new BScroll(this.$refs.wrapper, { // 声明为better-scroll
          probeType: this.probeType,
          click: this.click,
          bounce: this.bounce
        })
        if (this.listenScroll) {
          let self = this
          this.scroll.on('scroll', (pos) => {
            self.$emit('scroll', pos)
          })
        }
        if (this.pullUp) {
          this.scroll.on('scrollEnd', () => {
            if (this.scroll.y < (this.scroll.maxScrollY + 50)) {
              this.$emit('scrollToEnd')
            }
          })
        }
      },
      // better-scroll方法代理方法代理
      enable () {
        this.scroll && this.scroll.enable()
      },
      disable () {
        this.scroll && this.scroll.disable()
      },
      refresh () {
        this.scroll && this.scroll.refresh()
      },
      scrollTo () {
        this.scroll && this.scroll.scrollTo.apply(this.scroll, arguments)
      },
      scrollToElement () {
        this.scroll && this.scroll.scrollToElement.apply(this.scroll, arguments)
      }
    }
  }
</script>
