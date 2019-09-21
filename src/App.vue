<template>
  <div id="app">
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
