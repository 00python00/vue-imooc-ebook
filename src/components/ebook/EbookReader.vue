<template>
  <div class="ebook-reader">
     <div id="read">
     </div>
  </div>
</template>

<script>
 import { ebookMixin } from '../../utils/mixin'
  import Epub from 'epubjs'
 
  global.ePub = Epub
export default {
  mixins: [ebookMixin],
  methods:{
    // ...mapActions(['setMenuVisible']),
    prevPage() {
      if(this.rendition) {
        this.rendition.prev()
        this.hideTitleAndMenu()
      }
    },
    nextPage() {
      if (this.rendition) {
        this.rendition.next()
        this.hideTitleAndMenu()
      }
    },
    toggleTitleAndMenu() {
      this.$store.dispatch('setMenuVisible',!this.menuVisible);
      //this.setMenuVisible(!this.menuVisible)
    },
    hideTitleAndMenu(){
      this.$store.dispatch('setMenuVisible',false);
      //this.setMenuVisible(false)
    },
    initEpub(){
      const url = 'http://192.168.0.105:8081/epub/'+ this.fileName + '.epub'
      this.book = new Epub(url)
      this.rendition = this.book.renderTo('read',{
        width: window.innerWidth,
        height: window.innerHeight,
        method: 'default'
      })
      this.rendition.display()
      this.rendition.on('touchstart',event =>{
        this.touchStartX = event.changedTouches[0].clientX
        this.touchStartTime = event.timeStamp
      })
      this.rendition.on('touchend',event =>{
        const offsetX = event.changedTouches[0].clientX - this.touchStartX
        const time = event.timeStamp - this.touchStartTime
        this.ime = time
        if(time<500 && offsetX > 40) {
          this.prevPage()
        } else if (time<500 && offsetX < -40) {
          this.nextPage()
        } else {
          this.toggleTitleAndMenu()
        }
        event.preventDefault()
        event.stopPropagation() 
      })
    }
  },
  mounted(){
    
    const fileName = this.$route.params.fileName.split('|').join('/');
    this.$store.dispatch('setFileName', fileName).then(()=>{
      this.initEpub()
    })
    //  this.setFileName(fileName).then(()=>{
    //   this.initEpub()
    // })
  }
  
}
</script>

<style scoped lang='scss'>
  @import '../../assets/styles/global';
</style>