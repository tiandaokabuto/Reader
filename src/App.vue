<template>
  <div id="app">
    <span class="iconfont iconfanhui"></span>
    <span class="iconfont iconcaidan"></span>
    <img src="./assets/logo.png">
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App'
}
document.addEventListener('DOMContentLoaded', () => {
  const html = document.querySelector('html')
  let fontsize = window.innerWidth / 10
  fontsize = (fontsize > 50) ? 50 : fontsize
  html.style.fontSize = fontsize + 'px'
})
</script>

<style>

</style>
