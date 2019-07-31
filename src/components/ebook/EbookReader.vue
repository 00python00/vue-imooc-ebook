<template>
  <div class="ebook-reader">
     <div id="read">
     </div>
  </div>
</template>

<script>
  import { mapGetters } from 'vuex'
  import Epub from 'epubjs'
  global.ePub = Epub
export default {
  computed: {
    ...mapGetters(['fileName'])
  },
  methods:{
    initEpub(){
      const url = 'http://192.168.0.105:8081/epub/'+ this.fileName + '.epub'
      this.book = new Epub(url)
      this.rendition = this.book.renderTo('read',{
        width: window.innerWidth,
        height: window.innerHeight,
        method: 'default'
      })
      this.rendition.display()
    }
  },
  mounted(){
    
    const fileName = this.$route.params.fileName.split('|').join('/');
    this.$store.dispatch('setFileName', fileName).then(()=>{
      this.initEpub()
    })
   
  }
  
}
</script>

<style scoped lang='scss'>
  @import '../../assets/styles/global';
</style>