<template lang="pug">
  div.wrapper
    b-modal(
      title="GameOver!"
      hide-footer
      v-model="modalShow"
    )
      p.comment コメントに反応しよう！
      p.point {{clearCount*10}}point!!
      b-button(class="mt-3" @click="clickOK()" block) 閉じる
    b-navbar(toggleable type="light" variant="light")
      b-navbar-brand
        div.brand
          b-img.icon(src="https://pbs.twimg.com/media/D0LF_PuU8AAlevh.png" rounded)
          span NasTube
      b-navbar-brand
        span.point {{clearCount*10}}point
        b-img.icon(src="https://pbs.twimg.com/profile_images/1082257144492187649/rxDuGkcs_400x400.jpg" rounded="circle" alt="Circle image")
    div.main-content
      b-row
        b-col(cols="9").game-window
          b-row.live
            b-col.left(cols="3")
              b-img.effect-icon.thank-icon(
                :class="{active: thankActive}"
                src="../../static/img/thankyou.png"
              )
              b-img.effect-icon.ahecchi-icon(
                :class="{active: ahecchiActive}"
                src="../../static/img/ahecchi.png"
              )
            b-col.center(cols="6")
              b-img.natori(src="https://pbs.twimg.com/media/DrnaffbUcAAuclJ.png")
            b-col.right(cols="3")
              b-img.effect-icon.etti-icon(
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
            div.parson-box(v-for="cItem in commentItems" :key="cItem.uid" :class="[cItem.castomClass, {'success': cItem.clearFlag}]")
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
            b-button(size="sm" @click="buttonClick(1)" :class="{ 'shine-btn': btnShineJadgeThank }") ありがとう！
            b-button(size="sm" @click="buttonClick(0)" :class="{ 'shine-btn': btnShineJadgeEtti }") えっちじゃん
            b-button(size="sm" @click="buttonClick(2)" :class="{ 'shine-btn': btnShineJadgeAhecchi }") うれしいゾ～
</template>

<script>
export default {
  name: 'Game',
  data () {
    return {
      commentItems: [],
      clearCount: 0,
      stage: 1,
      hp: 100,
      max: 100,
      thankActive: false,
      ahecchiActive: false,
      ettiActive: false,
      modalShow: false,
      commentSpeed: 1000,
      templateComment: {
        'thank': ['¥3160', '¥710', '¥37', '¥3737'],
        'etti': ['今日のパンツの色何色', 'ふーんえっちじゃん', 'まぁ俺の方がエッチだけどね'],
        'ahecchi': ['１周年おめでと〜']
      },
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
      damageVoice: [
        '沼地の魔女こと名取さなですけども/ヴッ！',
        '沼地の魔女こと名取さなですけども/ヴッ！2',
        '沼地の魔女こと名取さなですけども/ヴッ！3'
      ],
      finVoice: [
        'せんせえがバーチャルさんを見ている時バーチャルさんもまた以下略/おわり～'
      ],
      buttonItem: [
        {
          name: 'etti',
          url: [
            '今日何の日か知ってる？/ふーん、エッチじゃん1',
            '今日何の日か知ってる？/ふーん、エッチじゃん2',
            '今日何の日か知ってる？/ふーん、エッチじゃん3',
            '今日何の日か知ってる？/ふーん、エッチじゃん4'
          ]
        },
        {
          name: 'thank',
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
          name: 'ahecchi',
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
    this.printComment()
  },
  methods: {
    playAudio (index) {
      const ad = new Audio()
      ad.addEventListener('loadedmetadata', () => {
        this.selectMoveAvtionIcon(index, ad.duration * 1000)
      })
      ad.src = this.throwVoiceURL(index)
      ad.play()
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
    ahecchiIconMoveAction (time) {
      this.ahecchiActive = true
      setTimeout(() => {
        this.ahecchiActive = false
      }, time)
    },
    ettiIconMoveAction (time) {
      this.ettiActive = true
      setTimeout(() => {
        this.ettiActive = false
      }, time)
    },
    selectMoveAvtionIcon (index, time) {
      const name = this.buttonItem[index].name
      if (name === 'thank') this.thankIconMoveActive(time)
      else if (name === 'etti') this.ettiIconMoveAction(time)
      else if (name === 'ahecchi') this.ahecchiIconMoveAction(time)
    },
    buttonClick (index) {
      const commentType = this.buttonItem[index].name
      const commnetIndex = this.firstObjectFind(commentType)
      if (commnetIndex !== -1) {
        this.playAudio(index)
        this.commentItems[ commnetIndex ].clearFlag = true
        this.clearCount++
      } else {
        this.damageHP()
      }
      if (this.clearCount % 5 === 0) {
        this.stage++
        this.commentSpeed /= 1.5
        if (this.commentSpeed < 100) this.commentSpeed = 100
      }
    },
    firstObjectFind (type) {
      const index = this.commentItems.findIndex(item => {
        return item.castomClass === type && !item.clearFlag
      }) || -1
      return index
    },
    resetHP () {
      this.hp = 100
    },
    clickOK () {
      this.resetHP()
      this.modalShow = !this.modalShow
      this.commentItems.splice(0)
      this.commentSpeed = 1000
      this.stage = 1
      this.clearCount = 0
      this.thankActive = false
      this.ahecchiActive = false
      this.ettiActive = false
    },
    damageHP () {
      if (this.hp > 0) {
        if (this.stage <= 5) this.hp -= 10
        else if (this.stage <= 10) this.hp -= 20
        else if (this.stage <= 15) this.hp -= 30
        else if (this.stage <= 20) this.hp -= 40
        else if (this.stage <= 30) this.hp -= 50
        let ad = new Audio()
        ad.src = this.makeVoiceUrl(this.damageVoice[Math.floor(Math.random() * this.damageVoice.length)])
        ad.play()
      }
    },
    createComentObjTemplate () {
      const iconIndex = Math.floor(Math.random() * this.faceIcon.length)
      return {
        icon: this.faceIcon[iconIndex],
        uid: Math.random().toString(36).slice(-8),
        name: Math.random().toString(36).slice(-8),
        castomClass: 'no-active',
        clearFlag: false
      }
    },
    printComment () {
      const random = Math.random()
      let displayComment = {}
      if (random > 0.7 && this.commentItems.length > 0) {
        displayComment = this.createComentObjTemplate()
        const keys = Object.keys(this.templateComment)
        const commentPatternChoiceIndex = Math.floor(Math.random() * keys.length)
        const commentList = this.templateComment[keys[commentPatternChoiceIndex]]
        const commentIndex = Math.floor(Math.random() * commentList.length)
        displayComment.comment = commentList[commentIndex]
        // customだけどめんどくさいからこのまま
        displayComment.castomClass = keys[commentPatternChoiceIndex]
      } else {
        displayComment = this.createComentObjTemplate()
        displayComment.comment = Math.random().toString(36).slice(-8)
      }
      this.commentItems.push(displayComment)
      setTimeout(this.printComment, this.commentSpeed)
    }
  },
  computed: {
    hpBarColor () {
      if (this.hp > 60) return this.progressBarColor.green
      else if (this.hp > 30) return this.progressBarColor.yellow
      else return this.progressBarColor.red
    },
    btnShineJadgeThank () {
      return this.commentItems.filter(item => {
        return item.castomClass === 'thank' && !item.clearFlag
      }).length > 0
    },
    btnShineJadgeEtti () {
      return this.commentItems.filter(item => {
        return item.castomClass === 'etti' && !item.clearFlag
      }).length > 0
    },
    btnShineJadgeAhecchi () {
      return this.commentItems.filter(item => {
        return item.castomClass === 'ahecchi' && !item.clearFlag
      }).length > 0
    }
  },
  watch: {
    commentItems (val) {
      const box = document.querySelector('.chat-window')
      const parson = document.querySelector('.parson-box')
      if (val.length > Math.floor(box.scrollHeight / parson.scrollHeight) - 1) {
        const removeItem = this.commentItems.shift()
        if (!removeItem.clearFlag && removeItem.castomClass !== 'no-active') this.damageHP()
      }
    },
    modalShow (val) {
      if (val <= 0 && val === false) this.resetHP()
    },
    hp (val) {
      if (val <= 0) {
        this.modalShow = !this.modalShow
        let ad = new Audio()
        ad.src = this.makeVoiceUrl(this.finVoice[Math.floor(Math.random() * this.finVoice.length)])
        ad.play()
      }
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
  background: #efefef;
}
.chat-box :hover{
  background: #bbbaba;
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
  border-radius: 10px;
}
.chat-box{
  font-weight: bold;
  border: solid 1px #000000;
  border-radius: 10px;
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
.thank{
  background-color: red;
}
.etti{
  background-color: aqua;
}
.ahecchi{
  background-color: burlywood;
}
.success{
  background-color: lightgreen;
}
@keyframes BLINK {
  0%{
    opacity: 1.0;
  }
  100% {
    opacity:0.5;
  }
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
  .chat-box{
    height: 75vh;
  }
  .game-window{
    height: 90vh;
  }
  .effect-icon{
    width: 25vh;
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
  height: 5vh;
  width: 100%;
}
.shine-btn{
  animation:BLINK 0.3s ease-in-out infinite alternate;
}
.hp-bar{
  position: absolute;
  bottom: 3vh;
  width: 95vw;
}
.point{
  padding: 10px;
  font-size: 150%;
}
</style>
