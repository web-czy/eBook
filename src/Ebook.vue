<template>
  <div class="ebook">
    <div class="read-wrapper">
      <div id="read"></div>
    </div>
  </div>
</template>

<script type='text/ecmascript-6'>
import Epub from 'epubjs'
const DOWNLOAD_URL = '/static/SuperTimeFinishing.epub'
// global.ePub = Epub
export default {
  methods: {
    // 电子书的解析和渲染
    showEpub() {
      // 生成Book
      this.book = Epub(DOWNLOAD_URL)
      // 生成Rendition,通过Book.renderTo
      this.rendition = this.book.renderTo('read', {
        width: window.innerWidth,
        height: window.innerHeight
      })
      // 通过Rendition.display渲染电子书
      this.rendition.display()
    }
  },
  mounted() {
    this.showEpub()
  }
}
</script>

<style scoped lang='stylus' rel='stylesheet/stylus'></style>
