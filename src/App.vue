<template>
  <div id="app">
    <guaguaka-ui ref="guaguaka" :guaguakaInfo="guaguakaInfo" id="guaguakaDiv" @success="guaguakaSuccess"></guaguaka-ui>
  </div>
</template>

<script>
import guaguakaUi from './myPlugin/guaguaka/index.vue'
export default {
  name: 'app',
  components: {
    guaguakaUi
  },
  data () {
    return {
      guaguakaInfo: {
        id: 'guaguakaDiv',
        width: 615,
        height: 330,
        isTouch: true,
        touchSpotSize: 20,
        visibleArea: 30,
        touchSpotShow: true,
        prizeImg: 'http://t2.hddhhn.com/uploads/tu/201610/198/scx30045vxd.jpg'
      }
    }
  },
  methods: {
    guaguakaSuccess (data) {
      console.log(data + '刮卡成功')
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
