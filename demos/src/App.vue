<template>
  <div id="app">
    <Tree :menuLists='menuLists' :index='index' @changeIndex="changeIndex"/>
  </div>
</template>

<script>
import Tree from './components/tree.vue'

export default {
  name: 'App',
  data() {
    return {
      menuLists: [
        {
          id: 1,
          name: '一级①',
          type: 1
        },{
          id: 2,
          name: '一级②',
          type: 1
        }
      ],
      index: -1
    }
  },
  components: {
    Tree
  },
  methods: {
    changeIndex(index) {
      this.index = index
      alert(index)
      this.$set(this.menuLists[index], 'childisshow', !this.menuLists[index].childisshow)
      console.log(this.menuLists)


    }
  },
  watch: {
    index(news) {
      console.log(news)
    }
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
}
#app {
  /* font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px; */
}
</style>
