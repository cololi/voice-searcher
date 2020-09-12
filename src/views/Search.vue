<template>
  <div class="search-wrapper">
    <div class="search">
      <transition name="logo">
        <div v-if="!showList" class="logo">
          <img src="../assets/image/vbup.svg">
        </div>
      </transition>
      <div class="input">
        <input type="text" v-model="value" :placeholder="$t('info.search')" @keyup.enter="search">
        <div class="clear">
          <transition name="fade">
            <svg @click="clear" style="cursor: pointer" t="1599129457149" v-if="value" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="7557" width="15" height="30"><path d="M510.4 67.2c-246.6 0-446.5 199.9-446.5 446.5s199.9 446.5 446.5 446.5 446.5-199.9 446.5-446.5S757 67.2 510.4 67.2z m223.9 670.5c-24.1 24.1-63.5 24.1-87.6 0L510.4 601.4 374 737.7c-24.1 24.1-63.5 24.1-87.6 0-24.1-24.1-24.1-63.5 0-87.6l136.3-136.3-136.3-136.4c-24.1-24.1-24.1-63.5 0-87.6 24.1-24.1 63.5-24.1 87.6 0l136.3 136.3 136.3-136.3c24.1-24.1 63.5-24.1 87.6 0 24.1 24.1 24.1 63.5 0 87.6L598 513.7 734.3 650c24.1 24.1 24.1 63.6 0 87.7z" p-id="7558" fill="#dddddd"></path></svg>
          </transition>
        </div>
        <button class="search-btn" @click="search">
          <img src="../assets/image/search.svg" style="width: 30px;height: 40px;margin-top: 3px;">
        </button>
      </div>
    </div>
    <div>
      <transition name="fade">
        <div v-if="showList">
          <hr>
          <div style="text-align: center;color: #ffffff;">结果数：{{searchList.length}}</div>
          <hr>
          <table style="color: #ffffff;">
            <tr>
              <th></th>
              <th>VTUBER</th>
              <th>中文翻译</th>
              <th>日本語翻訳</th>
              <th></th>
            </tr>
            <transition-group name="fade">
            <template v-for="(item, key) in searchList" :key="key">
              <tr>
                <td>{{key + 1}}</td>
                <td>{{item.author}}</td>
                <td>{{item.name['zh-CN'] ? item.name['zh-CN'] : '-'}}</td>
                <td>{{item.name['ja-JP'] ? item.name['ja-JP'] : '-'}}</td>
                <td>
                  <button @click="dlVoice(item.url)">下载</button>
                </td>
              </tr>
            </template>
            </transition-group>
            <tr v-if="searchList.length === 0">
                <td> - </td>
                <td> - </td>
                <td> - </td>
                <td> - </td>
                <td> - </td>
              </tr>
          </table>
        </div>
      </transition>
    </div>
  </div>
</template>

<script>
import { reactive, ref } from 'vue'
import VoiceList from '../../public/translate/voices.json'

export default {
  setup () {
    const data = []

    VoiceList.voices.forEach(item => {
      data.push({
        author: `${item.author}`,
        name: {
          'zh-CN': item.translate['zh-CN'],
          'ja-JP': item.translate['ja-JP']
        },
        url: `${location.origin}/voices/${item.category}/${item.path}`
      })
    })

    const value = ref('')
    const searchList = reactive([])

    const search = () => {
      searchList.length = 0
      if (value.value.length < 1) {
        showList.value = false
        return
      }
      data.forEach(item => {
        const reg = new RegExp(value.value, 'i')
        if ((item.author && reg.test(item.author)) || (item.name['zh-CN'] && reg.test(item.name['zh-CN'])) || (item.name['ja-JP'] && reg.test(item.name['ja-JP']))) {
          searchList.push(item)
        }
      })
      showList.value = true
    }

    const showList = ref(false)

    const dlVoice = (url) => {
      var eleLink = document.createElement('a')
      eleLink.download = ''
      eleLink.style.display = 'none'
      eleLink.href = url
      document.body.appendChild(eleLink)
      eleLink.click()
      document.body.removeChild(eleLink)
    }

    const clear = () => {
      value.value = ''
      searchList.length = 0
      showList.value = false
    }

    return {
      value,
      searchList,
      search,
      showList,
      dlVoice,
      clear
    }
  }

}
</script>

<style lang="stylus" scoped>
.search-wrapper
  min-height calc(100vh - 49px - 48px)
  background-color #333
  .search
    display flex
    flex-direction column
    align-items center
    justify-content center
    .logo
      overflow hidden
      display flex
      align-items center
      justify-content center
      height 100px
      max-width 50%
      margin-top 130px
      img
        width 100%
        margin auto
    .input
      display flex
      align-items center
      justify-content center
      margin 50px 0
      width 100%
      input
        box-sizing border-box
        font-size 20px
        height 50px
        width 60%
        border-radius 20px 0 0 20px
        padding 0 15px
        border-width 5px
        border #00ccff
        &:focus
          outline none
      .clear
        overflow hidden
        display flex
        align-items center
        justify-content center
        box-sizing border-box
        width 30px
        height 50px
        background #fff
        border 1px solid #ddd
        border-left none
        border-right none
        svg
          width 20px
      button
        background-color #00ccff
        height 50px
        width 50px
        border-radius 0 20px 20px 0
        border-width 5px
        border #00ccff
        border-left none
        cursor pointer
        &:focus
          outline none
        &:active
          background rgba(0, 204, 255, 0.8)
  table
    width 100%
    th
      padding 5px
    td
      text-align center

.logo-enter-active
  animation logo 0.5s
.logo-leave-active
  animation logo-leave 0.5s

@keyframes logo
  0%
    height 0
    margin-top 0
    opacity 0
  100%
    height 100px
    margin-top 150px
    opacity 1

@keyframes logo-leave
  0%
    height 100px
    margin-top 150px
    opacity 1
  100%
    height 0
    margin-top 0
    opacity 0
</style>
