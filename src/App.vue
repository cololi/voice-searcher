<template>
  <div id="app">
    <v-header />
    <router-view/>
    <control v-if="$route.path === '/'"/>
    <v-footer />
  </div>
</template>

<script>
import VHeader from './views/Header'
import VFooter from './views/Footer'
import { other } from '../public/translate/locales'
import { provide, reactive } from 'vue'

export default {
  components: {
    VHeader,
    VFooter
  },
  setup () {
    // 播放状态
    const setting = reactive({
      loading: true,
      error: false,
      nowPlay: null,
      overlap: false,
      autoRandom: false,
      loop: false
    })

    provide('setting', setting)

    console.log(`%c${other.consoleTip}%c `, `font-size:20px;color:${other.consoleTipColor}`, `padding-right:${other.consoleImgWidth};padding-top:${other.consoleImgHeight};background:url('${location.origin}/image/${other.consoleImg}') no-repeat;background-size:100% 100%`)
  }
}
</script>

<style lang="stylus">
@import '~@/assets/style/base.styl'

body
  margin 0
  -webkit-tap-highlight-color rgba(0, 0, 0, 0)
  background-color #333

a
  margin-left 5px
  color #888
  text-decoration none

::-webkit-scrollbar
  width 7px
  height 7px

::-webkit-scrollbar-track
  box-shadow inset 0 0 6px rgba(0, 0, 0, 0.3)
  -webkit-box-shadow inset 0 0 6px rgba(0, 0, 0, 0.3)
  background-color $main-color

::-webkit-scrollbar-thumb
  box-shadow inset 0 0 10px rgba(0, 0, 0, 0.5)
  -webkit-box-shadow inset 0 0 10px rgba(0, 0, 0, 0.5)
  background-color $sub-color

::-webkit-scrollbar-thumb:active
  background-color $main-color
</style>
