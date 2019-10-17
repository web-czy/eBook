<template>
  <transition name="slide-right">
    <div class="content">
      <div class="content-wrapper" v-if="bookAvailable">
        <scroll class="scroll" :data="navigation.toc">
          <div>
            <div
              class="content-item"
              v-for="(item, index) in navigation.toc"
              :key="index"
              @click="jumpTo(item.href)"
            >
              <span class="text">{{ item.label }}</span>
            </div>
          </div>
        </scroll>
      </div>
      <div class="empty" v-else>加载中...</div>
    </div>
  </transition>
</template>

<script type='text/ecmascript-6'>
import Scroll from 'base/scroll'

export default {
  props: {
    ifShowContent: Boolean,
    navigation: Object,
    bookAvailable: Boolean
  },
  methods: {
    jumpTo(target) {
      this.$emit('jumpTo', target)
    }
  },
  components: {
    Scroll
  }
}
</script>

<style scoped lang='scss'>
@import '~assets/styles/global';

.content {
  position: absolute;
  top: 0;
  left: 0;
  z-index: 102;
  width: 80%;
  height: 100%;
  background-color: #fff;
  .content-wrapper {
    width: 100%;
    height: 100%;
    padding: px2rem(10);
    box-sizing: border-box;
    .scroll {
      width: 100%;
      height: 100%;
    }
    .content-item {
      width: 100%;
      height: px2rem(26);
      line-height: px2rem(26);
      font-size: px2rem(12);
      color: #333;
      border-bottom: px2rem(1) solid #eee;
      &:last-child {
        border: none;
      }
    }
  }
  .empty {
    width: 100%;
    height: 100%;
    @include center;
    font-size: px2rem(12);
    color: #333;
  }
}
</style>
