<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
export default {

}
document.addEventListener('DOMContentLoaded', () => {
  const html = document.querySelector('html')
  let fontSzie = window.innerWidth / 10
  fontSzie = fontSzie > 50 ? 50 : fontSzie
  html.style.fontSize = fontSzie + 'px'
})
</script>
