<template>
  <div>
    <div @click="hideContextMenu">
      <v-btn @contextmenu.prevent="showContextMenu" dark>show menu<context-menu></context-menu></v-btn>
    </div>
  </div>
</template>
<script>
  import ContextMenu from './Context-menu.vue'
  export default {
    components: {
      ContextMenu
    },
    name: 'CategoriesContext',
    data () {
      return {
        contextMenuWidth: null,
        contextMenuHeight: null
      }
    },
    methods: {
      showContextMenu: () => {
        let menu = document.getElementById('context-menu')
        if (!this.contextMenuWidth || !this.contextMenuHeight) {
          menu.style.visibility = 'hidden'
          menu.style.display = 'block'
          this.contextMenuWidth = menu.offsetWidth
          this.contextMenuHeight = menu.offsetHeight
          menu.removeAttribute('style')
          console.log(menu)
        }

        if ((this.contextMenuWidth + event.pageX) >= window.innerWidth) {
          menu.style.left = (event.pageX - this.contextMenuWidth) + 'px'
        } else {
          menu.style.left = event.pageX + 'px'
        }

        if ((this.contextMenuHeight + event.pageY) >= window.innerHeight) {
          menu.style.top = (event.pageY - this.contextMenuHeight) + 'px'
        } else {
          menu.style.top = event.pageY + 'px'
        }
        menu.classList.add('active')
        console.log(menu)
      },
      hideContextMenu: () => {
        document.getElementById('context-menu').classList.remove('active')
      }
    }
  }
</script>
<!-- <style scoped>
  html, body, #app {
    margin: 0;
    width: 100%;
    height: 100%;
    display: flex;
    text-align: center;
    align-items: center;
    justify-content: center;
    background-color: #343338;
    font-family: Roboto, Tahoma, sans-serif;
  }

  .logo {
    height: 100px;
  }

  .instructions {
    color: white;
    display: block;
    font-size: 25px;
  }

  #context-menu {
    top: 0;
    left: 0;
    margin: 0;
    padding: 0;
    display: none;
    list-style: none;
    position: absolute;
    z-index: 2147483647;
    background-color: white;
    border: 1px solid #ebebeb;
    border-bottom-width: 0px;
  }

  #context-menu.active {
    display: block;
  }

  .context-menu-icon {
    top: 1px;
    position: relative;
    margin-right: 10px;
  }

  .context-menu-item {
    display: flex;
    cursor: pointer;
    padding: 8px 15px;
    align-items: center;
    border-bottom: 1px solid #ebebeb;
  }

  .context-menu-item:hover {
    background-color: #ebebeb;
  }
</style> -->
