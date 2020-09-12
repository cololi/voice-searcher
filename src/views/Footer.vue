<template>
  <div class="footer">
    <div>
      <div class="author">
        <div>©2020</div>
        <a href="https://github.com/blacktunes" target="_blank">Blacktunes</a>
        <a> & </a>
        <a href="https://github.com/VOOSC" target="_blank">VBUP</a>
      </div>
      <div class="info">
        <div>{{$i18n.messages[$i18n.locale].info.contact}}</div>
      </div>
    </div>
    <div class="text-right">
      <div class="git">
        <i-btn class="btn" :url="githubUrl" :img="require('../assets/image/github-fill.png')" />
      </div>
    </div>
  </div>
</template>

<script>
import info from '../../public/translate/locales.js'

export default {
  data () {
    return {
      githubUrl: 'https://github.com/blacktunes/kaguranana-button',
      info: info
    }
  }
}
</script>

<style lang="stylus" scoped>
.footer
  display flex
  justify-content space-between
  flex-wrap wrap
  padding 5px
  background-color #444444
  font-size 12px
  color #ddd
  .author
    display flex
    align-items center
  .info
    margin-top 5px
    line-height 18px
  .text-right
    text-align right
    .git
      display flex
      align-items center
      a
        margin 0
      .btn
        margin 0 5px

@media only screen and (max-width: 600px)
  .footer
    flex-direction column
    .text-right
      text-align left
      .git
        display flex
        align-items center
        .btn
          order 10
</style>
