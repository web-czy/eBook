<template>
  <div class="menu-bar">
    <transition name="slide-up">
      <div class="menu-wrapper" v-show="ifTitleAndMenuShow">
        <div class="icon-wrapper">
          <span class="iconfont icon-menu"></span>
        </div>
        <div class="icon-wrapper">
          <span class="iconfont icon-progress"></span>
        </div>
        <div class="icon-wrapper">
          <span class="iconfont icon-brightness"></span>
        </div>
        <div class="icon-wrapper" @click="toggleSettingShow">
          <span class="iconfont icon-a">A</span>
        </div>
      </div>
    </transition>
    <transition name="slide-up">
      <div class="setting-wrapper" v-show="ifTitleAndMenuShow && ifSettingShow">
        <div class="setting-font-size"></div>
      </div>
    </transition>
  </div>
</template>

<script type='text/ecmascript-6'>
export default {
  props: {
    ifTitleAndMenuShow: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      ifSettingShow: false
    }
  },
  methods: {
    toggleSettingShow() {
      this.ifSettingShow = !this.ifSettingShow
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
    width: 100%;
    height: px2rem(60);
    background: white;
    box-shadow: 0 px2rem(-8) px2rem(8) rgba(0, 0, 0, 0.15);
    .setting-font-size {
    }
  }
}
</style>
