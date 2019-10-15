<template>
  <transition name="slide-down">
    <div class="title-wrapper" v-show="ifTitleAndMenuShow">
      <div class="left">
        <span class="iconfont icon-xiangzuo"></span>
      </div>
      <div class="right">
        <div class="icon-wrapper">
          <span class="iconfont icon-gouwuche"></span>
        </div>
        <div class="icon-wrapper">
          <span class="iconfont icon-people"></span>
        </div>
        <div class="icon-wrapper">
          <span class="iconfont icon-gengduo"></span>
        </div>
      </div>
    </div>
  </transition>
</template>

<script type='text/ecmascript-6'>
export default {
  props: {
    ifTitleAndMenuShow: {
      type: Boolean,
      default: false
    }
  }
}
</script>

<style scoped lang='scss'>
@import '~assets/styles/global';

.title-wrapper {
  position: absolute;
  top: 0;
  left: 0;
  z-index: 101;
  display: flex;
  width: 100%;
  height: px2rem(48);
  background: white;
  box-shadow: 0 px2rem(8) px2rem(8) rgba(0, 0, 0, 0.15);
  .left {
    flex: 0 0 px2rem(60);
    @include center;
  }
  .right {
    flex: 1;
    display: flex;
    justify-content: flex-end;
    .icon-wrapper {
      // 每个小图标的宽度都为40
      flex: 0 0 px2rem(40);
      @include center;
      .icon-gouwuche {
        font-size: px2rem(22);
      }
    }
  }
}
</style>
