<template>
  <div class="content">
    <div class="content-wrapper" v-if="bookAvailable">
      <div class="content-item" v-for="(item, index) in navigation.toc" :key="index"
        @click="jumpTo(item.href)">
        <span class="text">{{item.label}}</span>
      </div>
    </div>
    <div class="empty" v-else>加载中...</div>
  </div>
</template>

<script>
export default {
  data () {
    return {

    }
  },
  props: {
    navigation: Object,
    bookAvailable: Boolean,
    ifContentShow: Boolean
  },
  methods: {
    jumpTo(href) {
      console.log(this.navigation)
      this.$emit('jumpTo', href)
    },
    mounted() {
      console.log(this.navigation)
    }
  }
}
</script>

<style lang="scss" scoped>
@import "../assets/styles/global";
.content {
  .content-wrapper {
    position: absolute;
    bottom: 0;
    left: 0;
    z-index: 102;
    display: flex;
    flex-direction: column;
    width: 70%;
    height: 100%;
    background: white;
    .content-item {
      flex: 1;
      width: 100%;
      height: px2rem(10);
      line-height: px2rem(10);
      padding-left: px2rem(15);
      .text {
        width: 100%;
        color: #333;
        cursor: pointer;
        font-size: px2rem(10);
      }
    }
  }
}
</style>
