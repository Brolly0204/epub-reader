<template>
  <div class="ebook">
    <title-bar :isTitleAndMenuShow="isTitleAndMenuShow"></title-bar>
    <div class="read-wrapper">
      <div id="read"></div>
      <div class="mask">
        <div
          class="left"
          @click="prevPage"
        ></div>
        <div
          class="center"
          @click="toggleTitleAndMenuShow"
        ></div>
        <div
          class="right"
          @click="nextPage"
        ></div>
      </div>
    </div>
    <menu-bar
      ref="menuBar"
      :isTitleAndMenuShow="isTitleAndMenuShow"
      :fontSizeList="fontSizeList"
      :defaultFontSize="defaultFontSize"
      @setFontSize="setFontSize"
      :defaultTheme="defaultTheme"
      :themeList="themeList"
      @setTheme="setTheme"
      :bookAvailable="bookAvailable"
      @onProgressChange="onProgressChange"
      :navigation="navigation"
      @jumpTo="jumpTo"
    ></menu-bar>
  </div>
</template>

<script>
import TitleBar from '@/components/TitleBar'
import MenuBar from '@/components/MenuBar'
import ePub from 'epubjs'

const BOOK_URL = '/static/2018_Book_AgileProcessesInSoftwareEngine.epub'
export default {
  components: {
    TitleBar,
    MenuBar
  },
  data() {
    return {
      isTitleAndMenuShow: false,
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
      bookAvailable: false,
      navigation: {}
    }
  },
  methods: {
    jumpTo(href) {
      this.rendition.display(href)
      this.hideTitleAndMenu()
    },
    hideTitleAndMenu() {
      this.isTitleAndMenuShow = false
      this.$refs.menuBar.hideSetting()
      this.$refs.menuBar.hideContent()
    },
    onProgressChange(progress) {
      const percentage = progress / 100
      const location = percentage > 0 ? this.locations.cfiFromPercentage(percentage) : 0
      this.rendition.display(location)
    },
    toggleTitleAndMenuShow() {
      this.isTitleAndMenuShow = !this.isTitleAndMenuShow
      if (!this.isTitleAndMenuShow) {
        this.$refs.menubar.hideSetting()
      }
    },
    showEpub() {
      this.book = ePub(BOOK_URL)
      this.rendition = this.book.renderTo('read', {
        width: window.innerWidth,
        height: window.innerHeight
      })
      this.rendition.display()
      this.themes = this.rendition.themes
      this.setFontSize(this.defaultFontSize)
      this.registerTheme()
      this.setTheme(0)
      this.book.ready.then(() => {
        this.navigation = this.book.navigation
        return this.book.locations.generate()
      }).then(result => {
        this.locations = this.book.locations
        this.bookAvailable = true
      })
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
    prevPage() {
      if (this.rendition) {
        this.rendition.prev()
      }
    },
    nextPage() {
      if (this.rendition) {
        this.rendition.next()
      }
    },
    setFontSize(fontSize) {
      if (this.themes) {
        this.defaultFontSize = fontSize
        this.themes.fontSize(fontSize + 'px')
      }
    }
  },
  mounted() {
    this.showEpub()
  }
}
</script>

<style lang="scss" scoped>
@import '~@/assets/styles/global.scss';
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
