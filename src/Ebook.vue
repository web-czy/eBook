<template>
  <div class="ebook">
    <title-bar :ifTitleAndMenuShow="ifTitleAndMenuShow"></title-bar>
    <div class="read-wrapper">
      <div id="read"></div>
      <div class="mask">
        <div class="left" @click="prevPage"></div>
        <div class="center" @click="toggleTitleAndMenuShow"></div>
        <div class="right" @click="nextPage"></div>
      </div>
    </div>
    <menu-bar ref="menuBar" :ifTitleAndMenuShow="ifTitleAndMenuShow"></menu-bar>
  </div>
</template>

<script type='text/ecmascript-6'>
import Epub from 'epubjs'
import TitleBar from './components/TitleBar'
import MenuBar from './components/MenuBar'

const DOWNLOAD_URL = '/static/SuperTimeFinishing.epub'
// global.ePub = Epub
export default {
  data() {
    return {
      ifTitleAndMenuShow: false
    }
  },
  methods: {
    toggleTitleAndMenuShow() {
      this.ifTitleAndMenuShow = !this.ifTitleAndMenuShow
      if (!this.ifTitleAndMenuShow) {
        this.$refs.menuBar.hideSettingShow()
      }
    },
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
    },
    prevPage() {
      // Rendition.prev
      if (this.rendition) {
        this.rendition.prev()
      }
    },
    nextPage() {
      // Rendition.next
      if (this.rendition) {
        this.rendition.next()
      }
    }
  },
  mounted() {
    this.showEpub()
  },
  components: {
    TitleBar,
    MenuBar
  }
}
</script>

<style scoped lang='scss'>
@import '~assets/styles/global';

.ebook {
  position: relative;
  .read-wrapper {
    .mask {
      position: absolute;
      top: 0;
      left: 0;
      z-index: 100;
      display: flex;
      width: 100%;
      height: 100%;
      .left {
        flex: 0 0 px2rem(100);
      }
      .center {
        flex: 1;
      }
      .right {
        flex: 0 0 px2rem(100);
      }
    }
  }
}
</style>
