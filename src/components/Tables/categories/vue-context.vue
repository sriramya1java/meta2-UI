<template>
  <div class="v-context"
       v-show="show"
       :style="style"
       tabindex="-1"
       @blur="close"
       @click="close"
       @contextmenu.capture.prevent
  >
    <slot :data="data"></slot>
  </div>
</template>

<script>
  export default {
    props: {
      closeOnScroll: {
        type: Boolean,
        default: true
      }
    },
    name: 'VueContext',
    computed: {
      /**
       * Generate the CSS styles for positioning the context menu.
       *
       * @returns {object|null}
       */
      style () {
        return this.show
          ? { top: `${this.top}px`, left: `${this.left}px` }
          : null
      }
    },

    data () {
      return {
        top: null,
        left: null,
        show: false,
        data: null,
        scrolled: false
      }
    },

    mounted () {
      if (this.closeOnScroll) {
        this.addScrollEventListener()
      }
    },

    beforeDestroy () {
      if (this.closeOnScroll) {
        this.removeScrollEventListener()
      }
    },

    methods: {
      /**
       * Add scroll event listener to close context menu.
       */
      addScrollEventListener () {
        window.addEventListener('scroll', this.close)
      },

      /**
       * Close the context menu.
       */
      close () {
        this.top = null
        this.left = null
        this.data = null
        this.show = false
      },

      /**
       * Open the context menu.
       *
       * @param model
       * @param {array|object|string} data User provided data for the menu
       */
      openContext (model, data) {
        this.data = data
        this.show = true
        console.log(model)
        let $evt = event
        this.$nextTick(() => {
          this.positionMenu($evt.clientY, $evt.clientX)
          this.$el.focus()
        })
      },

      /**
       * Set the context menu top and left positions.
       *
       * @param {number} top
       * @param {number} left
       */
      positionMenu (top, left) {
        const largestHeight = window.innerHeight - this.$el.offsetHeight - 25
        const largestWidth = window.innerWidth - this.$el.offsetWidth - 25

        if (top > largestHeight) {
          top = largestHeight
        }

        if (left > largestWidth) {
          left = largestWidth
        }

        this.top = top
        this.left = left
      },

      /**
       * Remove the scroll event listener to close the context menu.
       */
      removeScrollEventListener () {
        window.removeEventListener('scroll', this.close)
      },
      moveScroll () {
        // Any code to be executed when the window is scrolled
        console.log('scrolling....................................', this.scrolled)
        this.scrolled = window.scrollY > 0
        console.log('scrolling after ....................................', this.scrolled)
      }
    },

    watch: {
      /**
       * Add or remove the scroll event listener when the prop value changes.
       *
       * @param {boolean} value
       * @param {boolean} oldValue
       */
      closeOnScroll (value, oldValue) {
        if (value === oldValue) {
          return
        }

        if (value) {
          this.addScrollEventListener()
        } else {
          this.removeScrollEventListener()
        }
      }
    },
    created () {
      window.addEventListener('scroll', this.moveScroll)
    },
    destroyed () {
      window.removeEventListener('scroll', this.moveScroll)
    }
  }
</script>

<style lang="scss" scoped>
  $blue600: #1e88e5;
  $gray74: #bdbdbd;
  $gray93: #ededed;
  $gray98: #fafafa;

  .v-context {
    background: black;
    margin: 0;
    padding: 0;
    position: fixed;
    width: 10%;
    z-index: 99999;
    margin-left: 10%;

    ul {
      list-style: none;
      padding: 10px 0;
      margin: 0;
      font-size: 12px;
      font-weight: 600;

      li {
        margin: 0;
        padding: 10px 35px;
        cursor: pointer;

        &:hover {
          background: $blue600;
          color: $gray98;
        }
      }
    }
  }
</style>
