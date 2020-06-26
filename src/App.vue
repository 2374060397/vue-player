<template>
  <div id="app">
    <header>
      <h1>My Music</h1>
    </header>
    <main>
      <section class="player">
        <h2 class="song-title">{{ current.title }} - <span>{{ current.artist }}</span> </h2>
        <div class="controls">
          <button class="prev" @click="prev">Prev</button>
          <button class="play" v-if="!isPlaying" @click="play">Play</button>
          <button class="pause" v-else @click="pause">Pause</button>
          <!-- play和pause只有一个存在，当我点play，那么if会执行到play，play为false，pause才可以被点击 -->
          <button class="next" @click="next">Next</button>
        </div>
      </section>
      <section class="playlist">
        <h3>The PlayList</h3>
        <button v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == current.src) ? 'song playing' : 'song'">
          {{ song.title }} - {{ song.artist }}
        </button>
        
      </section>
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data () {
    return {
      current: {}, //虚拟的播放列表
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: 'bestofme',
          artist: 'Unkonw',
          src: require('./assets/BestOfMe.mp3')
        },
        {
          title: 'walls',
          artist: 'Unkonw1',
          src: require('./assets/walls.mp3')
        }
      ],
      player : new Audio() //生成一个播放器
    }
  },
  methods: {
    //播放

    play(song) {
      // console.log(this.song)
      if (typeof song.src !== "undefined"){
        this.current = song 
        this.player.src = this.current.src
      }
      this.player.play()
      this.player.addEventListener('ended', function() { //ended 事件在音频/视频(audio/video)播放完成后触发
        this.index++

        if(this.index > this.songs.length - 1) {
          this.index = 0
        }

        this.current = this.songs[this.index]
        this.play(this.current)
      }.bind(this))
      this.isPlaying = true
    },

    //暂停
    pause() {
      this.player.pause()
      this.isPlaying = false
    },

    //下一曲
    //index=0 那么++，就播放第二首，当下一首的歌到头了，那么回到第一首
    next() {
      this.index++
      if(this.index > this.songs.length - 1) {
        this.index = 0
      }

      this.current = this.songs[this.index]
      this.play(this.current)
    },

    //上一曲
    //同理，第一首回到最后一首
    prev() {
      this.index--
      if(this.index < 0) {
        this.index = this.songs.length - 1
      }

      this.current = this.songs[this.index]
      this.play(this.current)
    }

  },
  created () {
    // console.log(this.index) //0
    this.current = this.songs[this.index] //当组件被创建后，虚拟播放列表的的目录为第一首歌
    this.play.src = this.current.src //播放路径为当前的歌的地址
    // this.player.play()
  }
// 目前还需要完善的地方，歌名和作者名，这个可能需要接口会方便，还有一个是音量控制
}
</script>

<style>
* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
}
body {
	font-family: sans-serif;
}
header {
	display: flex;
	justify-content: center;
	align-items: center;
	padding: 15px;
	background-color: #212121;
	color: #FFF;
}
main {
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 25px;
}
.song-title {
  color: #53565A;
  font-size: 32px;
  font-weight: 700;
  text-transform: uppercase;
  text-align: center;
}
.song-title span {
  font-weight: 400;
  font-style: italic;
}
.controls {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 30px 15px;
}
button {
  appearance: none;
  background: none;
  border: none;
  outline: none;
  cursor: pointer;
}
button:hover {
  opacity: 0.8;
}
.play, .pause {
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
  border-radius: 8px;
  color: #FFF;
  background-color: #CC2E5D;
}
.next, .prev {
  font-size: 16px;
  font-weight: 700;
  padding: 10px 20px;
  margin: 0px 15px;
  border-radius: 6px;
  color: #FFF;
  background-color: #FF5858;
}
.playlist {
  padding: 0px 30px;
}
.playlist h3 {
  color: #212121;
  font-size: 28px;
  font-weight: 400;
  margin-bottom: 30px;
  text-align: center;
}
.playlist .song {
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 20px;
  font-weight: 700;
  cursor: pointer;
}
.playlist .song:hover {
  color: #FF5858;
}
.playlist .song.playing {
  color: #FFF;
  background-image: linear-gradient(to right, #CC2E5D, #FF5858);
}

</style>
