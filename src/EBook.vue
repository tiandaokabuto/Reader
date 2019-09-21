<template>
  <div class="page">
    <title-bar :showFlag="showFlag"></title-bar>
    <div class="reader-wrapper">
      <div id="reader"></div>
      <div class="mask">
        <div class="left" @click="prevPage"></div>
        <div class="center" @click="changeFlag"></div>
        <div class="right" @click="nextPage"></div>
      </div>
    </div>
    <menu-bar :showFlag="showFlag" ref="Menu"
    :fontSizeList="fontSizeList" :defaultFontSize="defaultFontSize"
    :themeList="themeList" :defaultTheme="defaultTheme"
    @setFontSize="setFontSize" @setTheme="setTheme"
    :bookAvailabel="bookAvailable" @onProgressChange="onProgressChange"
    :navigation="this.navigation" @jumpTo="jumpTo"></menu-bar>
  </div>
</template>

<script>
import Title from './components/Title'
import Menu from './components/Menu'
import Epub from 'epubjs'
const DOWNLOAD_URL = 'static/2018_Book_AgileProcessesInSoftwareEngine.epub'
export default {
  data() {
    return {
      showFlag: false,
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
          styles: {
            body: {
              'color': '#000',
              'background': '#fff'
            }
          }
        },
        {
          name: 'eye',
          styles: {
            body: {
              'color': '#000',
              'background': '#ceeaba'
            }
          }
        },
        {
          name: 'night',
          styles: {
            body: {
              'color': '#fff',
              'background': '#000'
            }
          }
        },
        {
          name: 'gold',
          styles: {
            body: {
              'color': '#000',
              'background': 'rgb(241, 236, 226)'
            }
          }
        }
      ],
      defaultTheme: 'eye',
      bookAvailable: false
    }
  },
  components: {
    TitleBar: Title,
    MenuBar: Menu
  },
  methods: {
    // 电子书的解析和渲染
    showEpub() {
      // 生成EBook对象
      const book = new Epub(DOWNLOAD_URL)
      // 生成rendition对象，第一个参数是DOM的id，第二个对象保存宽高
      this.rendition = book.renderTo('reader', {
        width: window.innerWidth,
        height: window.innerHeight
      })
      // 使用rendition对象渲染电子书
      this.rendition.display()
      // 获取Theme对象
      this.themes = this.rendition.themes
      // 设置默认字体
      this.themes.fontSize(this.defaultFontSize)
      // this.themes.register(name, styles) 注册主题
      // this.themes.select(name) 切换主题
      this.registeTheme()
      this.setTheme(this.defaultTheme)
      // 获取Location对象
      // 默认不会生成，需要通过epubjs的钩子函数来实现
      book.ready.then(() => {
        this.navigation = book.navigation
        return book.locations.generate()
      }).then((result) => {
        this.locations = book.locations
        this.bookAvailable = true
      })
    },
    // 上一页
    prevPage() {
      // rendition对象的prev方法
      if (this.rendition) {
        this.rendition.prev()
      }
    },
    // 下一页
    nextPage() {
      // rendition对象的next方法
      if (this.rendition) {
        this.rendition.next()
      }
    },
    // 切换标题栏和菜单栏的显示
    changeFlag() {
      // 隐藏标题栏菜单栏
      this.showFlag = false
      // 隐藏菜单栏弹出来的设置
      this.$refs.hideSetting()
      // 隐藏目录
      this.$refs.hideContent()
    },
    setFontSize(fontSize) {
      this.defaultFontSize = fontSize
      if (this.themes) {
        this.themes.fontSize(fontSize + 'px')
      }
    },
    registeTheme() {
      this.themeList.forEach((theme) => {
        this.themes.register(theme.name, theme.styles)
      })
    },
    setTheme(themeName) {
      this.themes.select(themeName)
      this.defaultTheme = themeName
    },
    // progress 进度条的变化 [0-100]
    onProgressChange(progress) {
      const percent = progress / 100
      const location = percent > 0 ? this.locations.cfiFromPercentage(percent) : 0
      this.rendition.display(location)
    },
    // 根据链接跳转到指定位置
    jumpTo(href) {
      this.rendition.display(href)
      this.changeFlag()
    }
  },
  mounted() {
    this.showEpub()
  }
}
</script>

<style lang="scss" scoped>
@import 'assets/styles/global';
.page {
  position: relative;
  .reader-wrapper {
    .mask {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: 100;
      display: flex;
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
