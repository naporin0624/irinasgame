<template lang="pug">
div.wrapper
  h1 名取のIRINASコメント反応ゲーム!
  h2 このサイトは名取さな様とは一切関係がありません
  p イラストダメだったら消します．できれば一番きれいな形で見ていただきたかったので・・・
  u.sorry
    b.sorry さなちゃんからいつも元気貰ってます、ありがとう！
  br
  u.sorry
    b.sorry 勝手に作っちゃってごめんなさい
  h2
    router-link(to="/game") 遊ぶ
  h2
    router-link(to="/about") 使い方
  h2
    router-link(to="/thanks") サンクス
  h2 ゲーム画面
  b-img(src="../../static/img/game1.PNG")

</template>

<style scoped>
 .sorry{
   text-align: left;
 }
</style>
