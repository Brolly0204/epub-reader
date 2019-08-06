<template>
  <transition name="slide-down">
    <div
      class="title-wrapper"
      v-show="isTitleAndMenuShow"
    >
      <div class="left">
        <span class="icon-back icon"></span>
      </div>
      <div class="right">
        <div class="icon-wrapper">
          <span class="icon-cart icon"></span>
        </div>
        <div class="icon-wrapper">
          <span class="icon-person icon"></span>
        </div>
        <div class="icon-wrapper">
          <span class="icon-more icon"></span>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  name: 'MenuBar',
  props: {
    isTitleAndMenuShow: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {}
  },
  components: {}
}
</script>

<style lang="scss" scoped>
@import '~@/assets/styles/global.scss';
.title-wrapper {
  display: flex;
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: px2rem(48);
  background-color: white;
  z-index: 102;
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
      flex: 0 0 px2rem(40);
      @include center;
      .icon-cart {
        font-size: px2rem(22);
      }
    }
  }
}
</style>
