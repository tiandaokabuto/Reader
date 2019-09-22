<template>
  <div class="menu-bar">
    <div class="menu-wrapper" v-show="showFlag" :class="{'hide-box-shadow':this.ifSettingShow}">
      <div class="icon-wrapper" @click="showSetting(0)">
        <span class="iconfont iconcaidan icon"></span>
      </div>
      <div class="icon-wrapper" @click="showSetting(1)">
        <span class="iconfont iconprogress icon"></span>
      </div>
      <div class="icon-wrapper" @click="showSetting(2)">
        <span class="iconfont iconliangdu icon"></span>
      </div>
      <div class="icon-wrapper" @click="showSetting(3)">
        <span class="icona icon">A</span>
      </div>
    </div>
    <div class="setting-wrapper" v-show="ifSettingShow">
      <div class="setting-fontsize" v-if="showTag === 3">
        <div class="preview" :style="{fontSize: fontSizeList[0].fontSize + 'px'}">A</div>
        <div class="select-wrapper-wrapper">
          <div class="select-wrapper" v-for="(item,index) in fontSizeList" :key="index" @click="setFontSize(item.fontSize)">
            <div class="line"></div>
            <div class="point-wrapper">
              <div class="point" v-show="defaultFontSize === item.fontSize">
                <div class="small-point"></div>
              </div>
            </div>
            <div class="line"></div>
          </div>
        </div>
        <div class="preview" :style="{fontSize: fontSizeList[6].fontSize + 'px'}">A</div>
      </div>
      <div class="setting-theme" v-else-if="showTag === 2">
        <div class="setting-theme-item" v-for="(item, index) in themeList" :key="index" @click="setTheme(item.name)">
          <div class="preview" :style="{background: item.styles.body.background}"
          :class="{'no-border': item.styles.body.background !== '#fff'}"></div>
          <div class="text">{{item.name}}</div>
        </div>
      </div>
      <div class="setting-progress" v-else-if="showTag === 1">
        <div class="progress-wrapper">
          <input type="range" class="progress"
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
          <span>{{bookAvailable ? progress + '%' : '加载中...'}}</span>
        </div>
      </div>
    </div>
    <content-view :ifContentShow="ifContentShow"
                  :navigation="navigation"
                  :bookAvailable="bookAvailable"
                  @jumpTo="jumpTo"
                  v-show="ifContentShow"></content-view>
    <div class="content-mask" v-show="ifContentShow" @click="hideContent"></div>
  </div>
</template>

<script>
import Content from './Content'
export default {
  props: {
    showFlag: Boolean,
    fontSizeList: Array,
    defaultFontSize: Number,
    themeList: Array,
    defaultTheme: String,
    bookAvailable: Boolean,
    navigation: Object
  },
  data() {
    return {
      ifSettingShow: false,
      showTag: 3,
      progress: 0,
      ifContentShow: false
    }
  },
  components: {
    ContentView: Content
  },
  methods: {
    showSetting(tag) {
      this.showTag = tag
      if (tag === 0) {
        this.ifSettingShow = false
        this.ifContentShow = true
      } else {
        this.ifSettingShow = true
      }
    },
    hideSetting() {
      this.ifSettingShow = false
    },
    hideContent() {
      this.ifContentShow = false
    },
    setFontSize(fontSize) {
      this.$emit('setFontSize', fontSize)
    },
    setTheme(themeName) {
      this.$emit('setTheme', themeName)
    },
    // 拖动进度条时触发的时间
    onProgressInput(progress) {
      this.progress = progress
      this.$refs.progress.style.backgroundSize = `${this.progress}% 100%`
    },
    onProgressChange(progress) {
      this.$emit('onProgressChange', progress)
    },
    jumpTo(href) {
      this.$emit('jumpTo', href)
    }
  }
}
</script>

<style lang="scss" scoped>
@import "../assets/styles/global";
.menu-bar{
  .menu-wrapper {
    position: absolute;
    bottom: 0;
    left: 0;
    width: 100%;
    height: px2rem(50);
    background: white;
    display: flex;
    z-index: 101;
    box-shadow: 0 px2rem(-10) px2rem(10) rgba(0, 0, 0, 0.2);
    &.hide-box-shadow {
      box-shadow: none;
    }
    .icon-wrapper {
      flex: 1;
      @include center;
      .iconliangdu {
        font-size: px2rem(30);
      }
      .iconprogress {
        font-size: px2rem(30);
      }
    }
  }
  .setting-wrapper {
    position: absolute;
    bottom: px2rem(50);
    left: 0;
    z-index: 101;
    width: 100%;
    height: px2rem(60);
    background: white;
    box-shadow: 0 px2rem(-10) px2rem(10) rgba(0, 0, 0, 0.2);
    .setting-fontsize {
      display: flex;
      height: 100%;
      .preview {
        flex: 0 0 px2rem(40);
        @include center;
      }
      .select-wrapper-wrapper {
        display: flex;
        flex: 1;
        .select-wrapper {
          flex: 1;
          display: flex;
          align-items: center;
          &:first-child {
            .line {
              &:first-child {
                border-top: none
              }
            }
          }
          &:last-child {
            .line {
              &:last-child {
                border-top: none
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
            height: px2rem(8);
            border-left: px2rem(1) solid #ccc;
            .point {
              position: absolute;
              top: px2rem(-7);
              left: px2rem(-8);
              height: px2rem(20);
              width: px2rem(20);
              border-radius: 50%;
              background: white;
              border: px2rem(1) solid #ccc;
              box-shadow: 0 px2rem(4) px2rem(4) rgba(0, 0, 0, 0.2);
              @include center;
              .small-point {
                height: px2rem(5);
                width: px2rem(5);
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
          font-size: px2rem(14);
          flex: 0 0 px2rem(20);
          color: #333;
          @include center;
        }
      }
    }
    .setting-progress {
      position: relative;
      widows: 100%;
      height: 100%;
      .progress-wrapper {
        width: 100%;
        height: 100%;
        padding: 0 px2rem(30);
        box-sizing: border-box;
        @include center;
        .progress {
          width: 100%;
          height: px2rem(2);
          -webkit-appearance: none;
          background: -webkit-linear-gradient(#999, #999) no-repeat #ddd;
          background-size: 0 100%;
          &::-webkit-slider-thumb {
            -webkit-appearance: none;
            height: px2rem(20);
            width: px2rem(20);
            border-radius: 50%;
            background: white;
            box-shadow: 0 4px 4px 0 rgba(0, 0, 0, 0.2);
            border: px2rem(1) solid #ddd;
          }
          &:focus {
            outline: none;
          }
        }
      }
      .text-wrapper {
        position: absolute;
        left: 0;
        bottom: 0;
        width: 100%;
        color: #333;
        font-size: px2rem(14);
        text-align: center;
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
    background: rgba(51, 51, 51, 0.8)
  }
}
</style>
