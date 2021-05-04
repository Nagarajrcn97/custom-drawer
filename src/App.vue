<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <button @click="open">OPEN DRAWER</button>
  <button @click="close">CLOSE DRAWER</button>
  <CustomDrawer :show="show" :close="close">
    <div>qwqwqwqw</div>
    <div>qwqwqwqw</div>
    <div>qwqwqwqw</div>
    <div>qwqwqwqw</div>
    <div>qwqwqwqw</div>
    <div>qwqwqwqw</div>
  </CustomDrawer>
</template>

<script>
import CustomDrawer from './components/CustomDrawer.vue'

export default {
  name: 'App',
  components: {
    CustomDrawer,
  },
  data: function() {
    return {
      show: false
    };
  },
  methods: {
    close() {
      this.show = false
    },
    open() {
      this.show = true
      console.log('open', this.show)
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
