<template>
  <div class="home">
    <span class="icon-headphones"></span>
    <!-- <div id="read">
    </div> -->
  </div>
</template>

<script>
// @ is an alias to /src
import Epub from 'epubjs'
global.epub = Epub

export default {
  // name: 'home',
  // mounted () {
  //   this.book = new Epub(`2017_Book_InterdisciplinaryPerspectivesO.epub`)
  //   this.book.renderTo('read', {
  //     width: window.innerWidth,
  //     height: window.innerHeight
  //   }).display()
  // }
}
</script>
<style scoped>
  .icon-headphones {
    font-size: 40px;
    color:red;
  }
</style>
