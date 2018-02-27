<template>
  <div id="app">
    <header>变量命名器</header>
    <main id="main">
      <el-container>
        <el-aside width="200px">
          <side-select @selectCange="selectCange"></side-select>
        </el-aside>
        <el-main>
          <main-content :seletedVal="selectValue"></main-content>
        </el-main>
      </el-container>
    </main>
    <footer>@释然</footer>
  </div>
</template>

<script>
  import sideSelect from './components/sideSelect'
  import mainContent from './components/mainCcontent'

  export default {
    name: 'App',
    data() {
      return {
        selectValue: null
      }
    },
    mounted() {
      this.computedHeight()
    },
    methods: {
      selectCange(value) {
        this.selectValue = value
      },
      computedHeight() {
        let H = window.innerHeight - 16
        let el = document.getElementById('app')
        el.style.height = H + 'px'
      }
    },
    components: {
      sideSelect,
      mainContent
    }
  }
</script>

<style scoped>
  #app {
    background-color: #E9EEF3;
  }

  #app header {
    background-color: #B3C0D1;
    color: #333;
    padding-left: 15%;
    line-height: 60px;
    font-size: 24px;
  }

  #main {
    color: #333;
    text-align: center;
    width: 80%;
    margin:100px auto 0;
  }
  #app footer {
    background-color: #d3dce6;
    color:#fff;
    text-align: center;
    position: absolute;
    bottom: 0;
    left: 0;
    right:0;
    padding:5px 0;
  }
</style>
