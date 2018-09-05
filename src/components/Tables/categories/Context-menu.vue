<template>
  <ul id="context-menu" class="context-menu-item">
    <li class="context-menu-item" v-on:click='newRegister'>
      New register
    </li>
    <li  class="context-menu-item"@click='remove'>
      Remove
    </li>
    <li class="context-menu-item" @click='edit'>
      Edit
    </li>
  </ul>

</template>
<script>
  import ContextMenuItem from './Context-menu-item.vue'
  export default {
    components: {
      ContextMenuItem
    },
    name: 'ContextMenu',
    props: {
      icon: ''
    },
    methods: {
      newRegister () {
        alert('New register')
        console.log('register method called')
      },
      remove () {
        alert('Remove')
      },
      edit () {
        alert('Edit')
      }
    }
  }
</script>
<style scoped>

  #context-menu {
    top: 0;
    left: 0;
    margin: 0;
    padding: 0;
    display: none;
    list-style: none;
    position: absolute;
    z-index: 2147483647;
    background-color: black;
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
    background-color: red;
  }
</style>
