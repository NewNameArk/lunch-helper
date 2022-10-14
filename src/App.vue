<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import HelloWorld from './components/HelloWorld.vue'
import imgUrl from './assets/logo.png'
</script>

<template>
  <img class="lunch-logo" alt="lunch logo" :src="imgUrl" />
  <HelloWorld msg="取餐天使创造工厂" />
</template>

<style lang="less">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  .lunch-logo {
    width: 200px;
  }
}
</style>
