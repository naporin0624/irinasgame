<template lang="pug">
  div.wrapper
    b-navbar(toggleable type="dark" variant="dark")
      b-navbar-brand
        div
          b-img.icon(src="https://pbs.twimg.com/media/D0LF_PuU8AAlevh.png" rounded)
          span NasTube
      b-navbar-brand
        b-img.icon(src="https://pbs.twimg.com/profile_images/1082257144492187649/rxDuGkcs_400x400.jpg" rounded="circle" alt="Circle image")
    div.maincontent
      b-row
        b-col(cols="9").movie-window
          b-row
            b-col(cols="3")
              p leftwindow
            b-col(cols="6")
              p centerWindow
            b-col(cols="3")
              p rightwindow
        b-col.chat-window
          div.chat-box
            div.parson-box(v-for="n in item" :key="n")
              b-row
                b-col(cols="1")
                  b-img.chat-icon(src="https://picsum.photos/125/125/?image=58" rounded="circle")
                b-col(cols="3")
                  span.name-box {{Math.random().toString(36).slice(-8)}}
                b-col
                  span.parson-box {{Math.random().toString(36).slice(-8)}}
          b-button-group.action-box
            b-button(
              size="sm"
              v-for="(item, index) in buttonItem"
              :key="item.name"
              @click="buttonClick(index)"
            ) {{item.name}}
</template>

<script>
export default {
  name: 'Game',
  data () {
    return {
      item: 20,
      ad: new Audio(),
      buttonItem: [
        {
          name: 'えっちじゃん',
          url: [
            '今日何の日か知ってる？/ふーん、エッチじゃん1',
            '今日何の日か知ってる？/ふーん、エッチじゃん2',
            '今日何の日か知ってる？/ふーん、エッチじゃん3',
            '今日何の日か知ってる？/ふーん、エッチじゃん4'
          ]
        },
        {
          name: 'ありがとう！',
          url: [
            '今日何の日か知ってる？/ありがと～1',
            '今日何の日か知ってる？/ありがと～2',
            '今日何の日か知ってる？/スパチャありがとう1',
            '今日何の日か知ってる？/ありがと～3',
            '今日何の日か知ってる？/ありがと～4',
            '今日何の日か知ってる？/スパチャありがとう2',
            '今日何の日か知ってる？/ありがと～5',
            '今日何の日か知ってる？/ありがと～6',
            '今日何の日か知ってる？/ありがと～7',
            '今日何の日か知ってる？/ありがと～8',
            '今日何の日か知ってる？/ありがと～9',
            '今日何の日か知ってる？/ありがと～10'
          ]
        },
        {
          name: 'うれしいゾ～',
          url: [
            '今日何の日か知ってる？/すっげえ嬉しかったゾ～',
            '今日何の日か知ってる？/すっげえ嬉しかったゾ～2'
          ]
        }
      ]
    }
  },
  updated () {
    this.$nextTick(() => {
      let container = document.querySelector('.chat-box')
      container.scrollTop = container.scrollHeight
    })
  },
  created () {
    this.increment()
  },
  methods: {
    increment () {
      this.item++
      setTimeout(this.increment, 1000)
    },
    playAudio (url) {
      this.ad.pause()
      this.ad.src = url
      this.ad.play()
    },
    makeVoiceUrl (name) {
      const url = 'https://www.natorisana.love/sounds/'
      return url + name + '.mp3'
    },
    buttonClick (index) {
      const voiceURList = this.buttonItem[index].url
      const choiceIndex = Math.floor(Math.random() * voiceURList.length)
      const url = this.makeVoiceUrl(voiceURList[choiceIndex])
      this.playAudio(url)
    }
  }
}
</script>

<style scoped>
.icon{
  width: 40px;
}
.maincontent{
  padding-top: 40px;
  padding-left: 80px;
  padding-right: 80px;
}
.chat-box{
  overflow-y: scroll;
}
.chat-icon{
  width: 30px;
}
.name-box{
  padding-top: 5px;
  padding-bottom: 5px;
  padding-left: 5px;
  padding-right: 5px;
}
.parson-box{
  padding-top: 5px;
  padding-bottom: 5px;
  padding-left: 5px;
  padding-right: 5px;
}
@media screen and (min-width: 1260px){
  .parson-box{
    width: 16vw;
  }
  .chat-box{
    height: 80vh;
  }
}
@media screen and (max-width: 1259px){
  .parson-box{
    width: 80vw;
  }
  .chat-box{
    height: 20vh;
  }
}
.text{
  word-wrap: break-word;
  text-align: left;
}
.action-box{
  padding: 10px;
}
</style>
