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
    <menu-bar
      ref="menuBar"
      :fontSizeList="fontSizeList"
      :defaultFontSize="defaultFontSize"
      :ifTitleAndMenuShow="ifTitleAndMenuShow"
      @setFontSize="setFontSize"
      :themeList="themeList"
      :defaultTheme="defaultTheme"
      @setTheme="setTheme"
      :bookAvailable="bookAvailable"
      @onProgressChange="onProgressChange"
      :navigation="navigation"
      @jumpTo="jumpTo"
    ></menu-bar>
  </div>
</template>

<script type='text/ecmascript-6'>
import Epub from 'epubjs'
import TitleBar from './components/TitleBar'
import MenuBar from './components/MenuBar'

const DOWNLOAD_URL = '/static/SuperTimeFinishing.epub'
global.ePub = Epub
export default {
  data() {
    return {
      ifTitleAndMenuShow: false,
      fontSizeList: [
        { fontSize: 12 },
        { fontSize: 14 },
        { fontSize: 16 },
        { fontSize: 18 },
        { fontSize: 20 },
        { fontSize: 22 },
        { fontSize: 24 }
      ],
      defaultFontSize: 16,
      themeList: [
        {
          name: 'default',
          style: {
            body: {
              color: '#000',
              background: '#fff'
            }
          }
        },
        {
          name: 'eye',
          style: {
            body: {
              color: '#000',
              background: '#ceeaba'
            }
          }
        },
        {
          name: 'night',
          style: {
            body: {
              color: '#fff',
              background: '#000'
            }
          }
        },
        {
          name: 'gold',
          style: {
            body: {
              color: '#000',
              background: 'rgb(241, 236, 226)'
            }
          }
        }
      ],
      defaultTheme: 0,
      // 图书是否处于可用状态
      bookAvailable: false,
      navigation: {}
    }
  },
  methods: {
    // 根据链接跳转到指定位置
    jumpTo(href) {
      this.rendition.display(href)
      this.hideTitleAndMenu()
    },
    hideTitleAndMenu() {
      // 隐藏标题栏和菜单栏
      this.ifTitleAndMenuShow = false
      // 隐藏菜单栏弹出的设置栏
      this.$refs.menuBar.hideSetting()
      // 隐藏目录
      this.$refs.menuBar.hideContent()
    },
    // progress 进度条的数值(0-100)
    onProgressChange(progress) {
      // 先把进度条变成百分比
      const percentage = progress / 100
      // 真实的页数location
      // percentage大于零再去获取页数
      // cfiFromPercentage()是获取位置的方法
      // 将percentage传入之后，就会自动生成location
      const location =
        percentage > 0 ? this.locations.cfiFromPercentage(percentage) : 0
      // display这里如果传入0，就会显示第一页
      this.rendition.display(location)
    },
    setTheme(index) {
      this.themes.select(this.themeList[index].name)
      this.defaultTheme = index
    },
    registerTheme() {
      this.themeList.forEach(theme => {
        this.themes.register(theme.name, theme.style)
      })
    },
    toggleTitleAndMenuShow() {
      this.ifTitleAndMenuShow = !this.ifTitleAndMenuShow
      if (!this.ifTitleAndMenuShow) {
        this.$refs.menuBar.hideSetting()
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
      // 获取Theme对象
      this.themes = this.rendition.themes
      // 设置默认字体
      this.setFontSize(this.defaultFontSize)
      // 注册主题
      // this.themes.register(name, styles)
      // 设置主题
      // this.themes.select(name)
      this.registerTheme()
      this.setTheme(this.defaultTheme)
      // 获取Locations对象
      // 通过epubjs的钩子函数来实现
      this.book.ready
        .then(() => {
          this.navigation = this.book.navigation
          return this.book.locations.generate()
        })
        .then(result => {
          this.locations = this.book.locations
          this.bookAvailable = true
        })
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
    },
    setFontSize(fontSize) {
      this.defaultFontSize = fontSize
      if (this.themes) {
        this.themes.fontSize(fontSize + 'px')
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
