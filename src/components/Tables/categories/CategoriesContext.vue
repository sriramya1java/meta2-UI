<template>
  <div>
    <div @click="hideContextMenu">
      <v-btn @contextmenu.prevent="showContextMenu" dark>show menu<context-menu></context-menu></v-btn>
    </div>
    <div>
      <p @contextmenu.prevent="$refs.menu.open">
        Right click on me
      </p>
    </div>
    <vue-context ref="menu">
      <ul>
        <li @click="eventClick(event.target.innerText)">Edit</li>
        <li @click="eventClick(event.target.innerText)">Delete</li>
      </ul>
    </vue-context>
  </div>
</template>
<script>
  import ContextMenu from './Context-menu.vue'
  import VueContext from './vue-context'

  export default {
    components: {
      ContextMenu,
      VueContext
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
        menu.style.left = '80px'
        menu.style.top = '40px'
        menu.classList.add('active')
        console.log(menu)
        console.log(menu.style.left)
        console.log(menu.style.top)
      },
      hideContextMenu: () => {
        document.getElementById('context-menu').classList.remove('active')
      }
    }
  }
</script>
