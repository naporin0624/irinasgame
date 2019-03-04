<template lang="pug">
  div.wrapper
    b-modal(
      title="GameOver!"
      hide-footer
      v-model="modalShow"
    )
      div.d-block.text-center
        h3 Hello From My Modal!
      p.comment コメントに反応しよう！
      p.point ポイント
      b-button(class="mt-3" @click="clickOK()" block) 閉じる
    b-alert.alert(show) Default Alert
    b-navbar(toggleable type="light" variant="light")
      b-navbar-brand
        div.brand
          b-img.icon(src="https://pbs.twimg.com/media/D0LF_PuU8AAlevh.png" rounded)
          span NasTube
      b-navbar-brand
        b-img.icon(src="https://pbs.twimg.com/profile_images/1082257144492187649/rxDuGkcs_400x400.jpg" rounded="circle" alt="Circle image")
    div.main-content
      b-row
        b-col(cols="9").game-window
          b-row.live
            b-col.left(cols="3")
              b-img.effect-icon.thank-you(
                :class="{active: thankActive}"
                src="../../static/img/thankyou.png"
              )
              b-img.effect-icon.aheebi(
                :class="{active: aheebiActive}"
                src="../../static/img/aheebi.png"
              )
            b-col.center(cols="6")
              b-img.natori(src="https://pbs.twimg.com/media/DrnaffbUcAAuclJ.png")
            b-col.right(cols="3")
              b-img.effect-icon.etti(
                :class="{ active: ettiActive }"
                src="../../static/img/etti.png"
              )
          div.hp-bar
            span HP
            b-progress(
              :value="hp"
              :max="max"
              show-progress
              :animated="!modalShow"
              :variant="hpBarColor"
            )
        b-col.chat-window
          div.chat-box
            div.parson-box(v-for="cItem in commentItems" :key="cItem.uid")
              b-row
                b-col(cols="1")
                  b-img.chat-icon(
                    :src="cItem.icon"
                    rounded="circle")
                b-col(cols="3")
                  span.name-box {{cItem.name}}
                b-col
                  span.parson-box {{cItem.comment}}
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
      commentItems: [],
      stage: 10,
      hp: 100,
      max: 100,
      thankActive: false,
      aheebiActive: false,
      ettiActive: false,
      modalShow: false,
      progressBarColor: {
        'green': 'success',
        'red': 'danger',
        'yellow': 'warning'
      },
      faceIcon: [
        'https://3.bp.blogspot.com/-KgUzGDeV8r8/VaMOD3z_X-I/AAAAAAAAvh8/YK5LucKKUmo/s170/boy_01.png',
        'https://2.bp.blogspot.com/-H3-fqqm8qWo/VaMOEUvMmvI/AAAAAAAAvh4/_Y4PrsjulSw/s800/boy_02.png',
        'https://2.bp.blogspot.com/-D_mv2QF9rCg/VaMOFUh9EhI/AAAAAAAAviM/7_m2W2hR1TY/s800/boy_03.png',
        'https://3.bp.blogspot.com/-uRt9-Gjitio/VaMOFspgKCI/AAAAAAAAviI/7sdXUVAAzlE/s800/boy_04.png',
        'https://3.bp.blogspot.com/-jtvaRX9n8OU/VaMOHGaVeRI/AAAAAAAAviQ/D9omX3FtbPw/s800/boy_05.png',
        'https://4.bp.blogspot.com/-eDLar-FeZ6Q/VaMOHWznrOI/AAAAAAAAvic/MHSkHPvstPI/s800/boy_06.png',
        'https://3.bp.blogspot.com/-3CtFNaO7nZQ/VaMNdpSHc8I/AAAAAAAAvYA/qFFLFrGkrjo/s800/girl_13.png',
        'https://2.bp.blogspot.com/-6JmvIZNh6v0/VaMNdoZ16HI/AAAAAAAAvX8/26ub6j9Uh4E/s800/girl_14.png',
        'https://2.bp.blogspot.com/-b5xF1YTPEo4/VaMNfzznp9I/AAAAAAAAvYk/ByubINYOiXI/s800/girl_20.png',
        'https://2.bp.blogspot.com/-_2T6ZwRQPso/VaMNfL52n-I/AAAAAAAAvZc/PS91_JU-DOo/s800/girl_18.png',
        'https://3.bp.blogspot.com/-VGG_G8NltMA/VaMNiDYa-UI/AAAAAAAAvZA/fxGBCmKG_Ug/s800/girl_24.png',
        'https://2.bp.blogspot.com/-KtkAeTZWRpQ/VaMNg2zA-gI/AAAAAAAAvYw/46beWDLw4GA/s800/girl_22.png'
      ],
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
      const container = document.querySelector('.chat-box')
      container.scrollTop = container.scrollHeight
    })
  },
  created () {
    this.createComent()
  },
  methods: {
    playAudio (index) {
      const ad = new Audio()
      ad.addEventListener('loadedmetadata', () => {
        this.selectMoveAvtionIcon(index, ad.duration * 1000)
      })
      ad.src = this.throwVoiceURL(index)
      ad.play()
      this.damageHP()
    },
    makeVoiceUrl (name) {
      const url = 'https://www.natorisana.love/sounds/'
      return url + name + '.mp3'
    },
    throwVoiceURL (index) {
      const voiceURList = this.buttonItem[index].url
      const choiceIndex = Math.floor(Math.random() * voiceURList.length)
      const url = this.makeVoiceUrl(voiceURList[choiceIndex])
      return url
    },
    thankIconMoveActive (time) {
      this.thankActive = true
      setTimeout(() => {
        this.thankActive = false
      }, time)
    },
    aheebiIconMoveAction (time) {
      this.aheebiActive = true
      setTimeout(() => {
        this.aheebiActive = false
      }, time)
    },
    ettiIconMoveAction (time) {
      this.ettiActive = true
      setTimeout(() => {
        this.ettiActive = false
      }, time)
    },
    selectMoveAvtionIcon (index, time) {
      console.log('run selectMovdActionItem', index, time)
      const name = this.buttonItem[index].name
      if (name === 'ありがとう！') this.thankIconMoveActive(time)
      else if (name === 'えっちじゃん') this.ettiIconMoveAction(time)
      else if (name === 'うれしいゾ～') this.aheebiIconMoveAction(time)
    },
    buttonClick (index) {
      this.playAudio(index)
    },
    resetHP () {
      this.hp = 100
    },
    clickOK () {
      this.resetHP()
      this.modalShow = !this.modalShow
    },
    damageHP () {
      if (this.hp > 0) {
        if (this.stage <= 5) this.hp -= 10
        else if (this.stage <= 10) this.hp -= 20
        else if (this.stage <= 15) this.hp -= 30
        else if (this.stage <= 20) this.hp -= 40
        else if (this.stage <= 30) this.hp -= 50
      }
    },
    createComent () {
      const iconIndex = Math.floor(Math.random() * this.faceIcon.length)
      this.commentItems.push({
        icon: this.faceIcon[iconIndex],
        uid: Math.random().toString(36).slice(-8),
        name: Math.random().toString(36).slice(-8),
        comment: Math.random().toString(36).slice(-8)
      })
      setTimeout(this.createComent, 1000)
    }
  },
  computed: {
    hpBarColor () {
      if (this.hp > 60) return this.progressBarColor.green
      else if (this.hp > 30) return this.progressBarColor.yellow
      else return this.progressBarColor.red
    }
  },
  watch: {
    item (val) {
      const box = document.querySelector('.chat-window')
      const parson = document.querySelector('.parson-box')
      if (val > Math.floor(box.scrollHeight / parson.scrollHeight)) this.commentItems.shift()
    },
    modalShow (val) {
      if (val <= 0 && val === false) this.resetHP()
    },
    hp (val) {
      if (val <= 0) this.modalShow = !this.modalShow
    }
  }
}
</script>

<style scoped>
.alert{
  position: absolute;
  left: 0px;
  top: 100px;
}
.icon{
  width: 40px;
}
.main-content{
  padding-top: 20px;
  padding-left: 2vw;
  padding-right: 2vw;
}
.chat-box{
  overflow-y: scroll;
}
.chat-icon{
  width: 30px;
}
.name-box{
  padding: 5px;
}
.parson-box{
  padding: 5px;
}
.live{
  font-weight: bold;
  background-color: rgb(56, 56, 56);
}
.chat-box{
  font-weight: bold;
  border: solid 1px #000000;
}
.center{
  background-image: url(
    'https://3.bp.blogspot.com/-wrWa_T7i7u4/VpjBoMACY-I/AAAAAAAA248/GLflyRpwvEs/s1600/bg_hospital_chiryou.jpg'
  )
}
.natori{
  width: 25vw;
  z-index: 0;
}
.effect-icon{
  position: relative;
  z-index: 1000;
}
.active{
  animation: updown .5s infinite;
}
@keyframes idou {
  100%{
    transform: translateX(25vw) translateY(25vh);
  }
}
@keyframes updown {
  0%{transform: translateY(0px)}
  25%{transform: translateY(20px)}
  50%{transform: translateY(0px)}
  75%{transform: translateY(20px)}
  100%{transform: translateY((0px))}
}
@media screen and (min-width: 1260px){
  .live{
    height: 80vh;
  }
  .parson-box{
    width: 16vw;
  }
  .chat-box{
    height: 72vh;
  }
  .game-window{
    height: 90vh;
  }
  .effect-icon{
    width: 30vh;
  }
  .center{
    height: 80vh;
  }
  .natori{
    width: 50vh;
    position:absolute;
    left:0;
    right:0;
    bottom:0;
    margin:auto;
  }
}
@media screen and (max-width: 1259px){
  .live{
    height: 50vh;
    width: 95vw;
  }
  .parson-box{
    width: 90vw;
  }
  .chat-box{
    height: 20vh;
  }
  .game-window{
    height: 60vh;
  }
  .effect-icon{
    width: 15vh;
  }
  .center{
    height: 50vh;
  }
  .natori{
    width: 30vh;
    position:absolute;
    left:0;
    right:0;
    bottom:0;
    margin:auto;
  }
}
.text{
  word-wrap: break-word;
  text-align: left;
}
.action-box{
  padding: 10px;
}
.hp-bar{
  position: absolute;
  bottom: 3vh;
  width: 95vw;
}
</style>
