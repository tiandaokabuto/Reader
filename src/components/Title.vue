<template>
  <div class="title-bar">
    <transition>
      <div class="title-wrapper" v-show='showFlag'>
        <div class="left">
          <span class="iconfont iconfanhui icon"></span>
        </div>
        <div class="right">
          <div class="icon-wrapper">
            <div class="iconfont iconcart_icon icon"></div>
          </div>
          <div class="icon-wrapper">
            <div class="iconfont icongeren icon"></div>
          </div>
          <div class="icon-wrapper">
            <div class="iconfont icondashujukeshihuaico- icon"></div>
          </div>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
export default {
  props: {
    showFlag: Boolean
  }
}
</script>

<style lang="scss" scoped>
@import '../assets/styles/global';
.title-wrapper {
  position: absolute;
  z-index: 101;
  top: 0;
  left: 0;
  width: 100%;
  height: px2rem(50);
  background: white;
  display: flex;
  box-shadow: 0 px2rem(10) px2rem(10) rgba(0, 0, 0, 0.2);
  justify-content: center;
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
      .icondashujukeshihuaico- {
        font-size: px2rem(35)
      }
    }
  }
}
</style>
