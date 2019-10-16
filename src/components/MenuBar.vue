<template>
  <div class="menu-bar">
    <transition name="slide-up">
      <div
        class="menu-wrapper"
        :class="{ 'hide-box-shadow': ifSettingShow || !ifTitleAndMenuShow }"
        v-show="ifTitleAndMenuShow"
      >
        <div class="icon-wrapper">
          <span class="iconfont icon-menu" @click="showContent"></span>
        </div>
        <div class="icon-wrapper" @click="showSetting(2)">
          <span class="iconfont icon-progress"></span>
        </div>
        <div class="icon-wrapper" @click="showSetting(1)">
          <span class="iconfont icon-brightness"></span>
        </div>
        <div class="icon-wrapper" @click="showSetting(0)">
          <span class="iconfont icon-a">A</span>
        </div>
      </div>
    </transition>
    <transition name="slide-up">
      <div class="setting-wrapper" v-show="ifTitleAndMenuShow && ifSettingShow">
        <div class="setting-font-size" v-if="showTag === 0">
          <div
            class="preview"
            :style="{
              fontSize: fontSizeList[0].fontSize + 'px',
              paddingLeft: deviation + 'px'
            }"
            ref="preview"
          >
            A
          </div>
          <div class="select">
            <div
              class="select-wrapper"
              v-for="(item, index) in fontSizeList"
              :key="index"
              @click="setFontSize(item.fontSize)"
            >
              <div class="line"></div>
              <div class="point-wrapper">
                <div class="point" v-show="defaultFontSize === item.fontSize">
                  <div class="small-point"></div>
                </div>
              </div>
              <div class="line"></div>
            </div>
          </div>
          <div
            class="preview"
            :style="{
              fontSize: fontSizeList[fontSizeList.length - 1].fontSize + 'px',
              paddingRight: deviation + 'px'
            }"
          >
            A
          </div>
        </div>
        <div class="setting-theme" v-else-if="showTag === 1">
          <div
            class="setting-theme-item"
            v-for="(item, index) in themeList"
            :key="index"
            @click="setTheme(index)"
          >
            <div
              class="preview"
              :style="{ background: item.style.body.background }"
              :class="{ 'no-border': item.style.body.background !== '#fff' }"
            ></div>
            <div class="text" :class="{ selected: index === defaultTheme }">
              {{ item.name }}
            </div>
          </div>
        </div>
        <div class="setting-progress" v-else-if="showTag === 2">
          <div class="progress-wrapper">
            <input class="progress"
                    type="range"
                    max="100"
                    min="0"
                    step="1"
                    @change="onProgressChange($event.target.value)"
                    @input="onProgressInput($event.target.value)"
                    :value="progress"
                    :disabled="!bookAvailable"
                    ref="progress">
          </div>
          <div class="text-wrapper">
            <span>{{ bookAvailable ? progress + '%' : '加载中...'}}</span>
          </div>
        </div>
      </div>
    </transition>
    <transition name="fade">
      <div class="content-mask" v-show="ifShowContent" @click="hideContent"></div>
    </transition>
  </div>
</template>

<script type='text/ecmascript-6'>
export default {
  props: {
    ifTitleAndMenuShow: {
      type: Boolean,
      default: false
    },
    fontSizeList: {
      type: Array,
      default() {
        return []
      }
    },
    defaultFontSize: Number,
    themeList: {
      type: Array,
      default() {
        return []
      }
    },
    defaultTheme: Number,
    bookAvailable: {
      type: Boolean,
      default: false
    },
    navigation: {
      type: Object,
      default() {
        return {}
      }
    }
  },
  data() {
    return {
      ifSettingShow: false,
      deviation: 0,
      showTag: 0,
      progress: 0,
      ifShowContent: false
    }
  },
  methods: {
    showContent() {
      this.ifShowContent = true
    },
    hideContent() {
      this.ifShowContent = false
    },
    jumpTo(target) {
      this.$emit('jumpTo', target)
    },
    // 拖动进度条时触发事件
    onProgressInput(progress) {
      this.progress = progress
      this.$refs.progress.style.backgroundSize = `${this.progress}% 100%`
    },
    // 进度条松开后触发事件，根据进度条数值跳转到指定位置
    onProgressChange(progress) {
      this.$emit('onProgressChange', progress)
    },
    setTheme(index) {
      this.$emit('setTheme', index)
    },
    setFontSize(fontSize) {
      this.$emit('setFontSize', fontSize)
    },
    showSetting(tag) {
      this.ifSettingShow = true
      this.showTag = tag
    },
    hideSetting() {
      this.ifSettingShow = false
    }
  },
  watch: {
    ifSettingShow() {
      let winWidth = window.innerWidth
      let htmlSize = parseInt(document.querySelector('html').style.fontSize)
      let previewWidth = (40 / 37.5) * htmlSize
      this.deviation =
        (winWidth - previewWidth * 2) / (this.fontSizeList.length * 2) / 2 + 6
    }
  }
}
</script>

<style scoped lang='scss'>
@import '~assets/styles/global';

.menu-bar {
  .menu-wrapper {
    position: absolute;
    bottom: 0;
    left: 0;
    z-index: 101;
    display: flex;
    width: 100%;
    height: px2rem(48);
    background: white;
    box-shadow: 0 px2rem(-8) px2rem(8) rgba(0, 0, 0, 0.15);
    &.hide-box-shadow {
      box-shadow: none;
    }
    .icon-wrapper {
      width: 25%;
      // flex: 1;
      @include center;
      .icon-progress {
        font-size: px2rem(26);
      }
      .icon-brightness {
        font-size: px2rem(24);
      }
    }
  }
  .setting-wrapper {
    position: absolute;
    bottom: px2rem(48);
    left: 0;
    z-index: 101;
    width: 100%;
    height: px2rem(60);
    background: white;
    box-shadow: 0 px2rem(-8) px2rem(8) rgba(0, 0, 0, 0.15);
    .setting-font-size {
      display: flex;
      height: 100%;
      .preview {
        flex: 0 0 px2rem(40);
        @include center;
        box-sizing: border-box;
      }
      .select {
        flex: 1;
        display: flex;
        .select-wrapper {
          flex: 1;
          display: flex;
          align-items: center;
          &:first-child {
            .line {
              &:first-child {
                border: none;
              }
            }
          }
          &:last-child {
            .line {
              &:last-child {
                border: none;
              }
            }
          }
          .line {
            flex: 1;
            height: 0;
            border-top: px2rem(1) solid #ccc;
          }
          .point-wrapper {
            position: relative;
            flex: 0 0 0;
            width: 0;
            height: px2rem(7);
            border-left: px2rem(1) solid #ccc;
            .point {
              position: absolute;
              top: px2rem(-5);
              left: px2rem(-9);
              width: px2rem(15);
              height: px2rem(15);
              border-radius: 50%;
              background: #fff;
              border: px2rem(1) solid #ccc;
              box-shadow: 0 px2rem(4) px2rem(4) rgba(0, 0, 0, 0.15);
              @include center;
              .small-point {
                width: px2rem(5);
                height: px2rem(5);
                background: #000;
                border-radius: 50%;
              }
            }
          }
        }
      }
    }
    .setting-theme {
      height: 100%;
      display: flex;
      .setting-theme-item {
        flex: 1;
        display: flex;
        flex-direction: column;
        padding: px2rem(5);
        box-sizing: border-box;
        .preview {
          flex: 1;
          border: px2rem(1) solid #ccc;
          box-sizing: border-box;
          &.no-border {
            border: none;
          }
        }
        .text {
          flex: 0 0 px2rem(20);
          font-size: px2rem(14);
          color: #ccc;
          @include center;
          &.selected {
            color: #333;
          }
        }
      }
    }
    .setting-progress {
      position: relative;
      width: 100%;
      height: 100%;
      .progress-wrapper {
        width: 100%;
        height: 100%;
        @include center;
        padding: 0 px2rem(30);
        box-sizing: border-box;
        .progress {
          width: 100%;
          // 通过下边这个来覆盖默认样式
          -webkit-appearance: none;
          height: px2rem(2);
          // 左侧999 右侧ddd
          background-color: #ddd;
          // 如果拖到25%的时候，前边999，后边ddd，就是background-size: 25% 100%
          // 需要通过函数方法来实现
          background-size: 0 100%;
          &:focus {
            outline: none;
          }
          // 进度条上的手柄
          &::-webkit-slider-thumb {
            -webkit-appearance: none;
            width: px2rem(20);
            height: px2rem(20);
            border-radius: 50%;
            background: #fff;
            box-shadow: 0 4px 4px 0 rgba(0, 0, 0, 0.15);
            border: px2rem(1) solid #ddd;
          }
        }
      }
      .text-wrapper {
        position: absolute;
        left: 0;
        bottom: 0;
        width: 100%;
        text-align: center;
        font-size: px2rem(12);
        color: #333;
      }
    }
  }
  .content-mask {
    position: absolute;
    top: 0;
    left: 0;
    z-index: 101;
    display: flex;
    width: 100%;
    height: 100%;
    background: rgba(51, 51, 51, 0.8);
  }
}
</style>
