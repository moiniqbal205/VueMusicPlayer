<template>
  <div id="app">
    <header>
      <h1>MI<span>Tunes</span></h1>
    </header>

    <main>
      <section class="player">
        <h2 class="song-title">
          {{ current.title }} <br/> 
          <span>{{ current.artist }}</span>
        </h2>
        <div class="controls">
          <button class="prev" @click="prev">
            <i class="material-icons">skip_previous</i>
          </button>
          <button class="play" v-if="!isPlaying" @click="play">
            <i class="material-icons">play_circle_outline</i>
          </button>
          <button class="pause" v-else @click="pause">
            <i class="material-icons">pause_circle_outline</i>
          </button>
          <button class="next" @click="next">
            <i class="material-icons">skip_next</i>
          </button>
        </div>
      </section>

      <section class="playlist">
        <h3>Playlist</h3>
        <button 
          v-for="song in songs" 
          :key="song.src" 
          @click="play(song)" 
          :class="(song.src == current.src) ? 'song playing' : 'song'"
        >
          {{ song.title }} <br/> 
          <span class="song-artist">{{ song.artist }}</span>
        </button>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: 'Take a look around',
          artist: 'Limp Bizkit',
          src: require('./assets/Limp Bizkit - Now I Know Why You Wanna Hate Me ( Take a look around ).mp3')
        },
        {
          title: 'This is war',
          artist: 'Zayde Wolf',
          src: require('./assets/this-is-war-feat-richard-farrell.mp3')
        },
        {
          title: 'Rule the world',
          artist: 'Zayde Wolf',
          src: require('./assets/zayde-wolf-rule-the-world-lyric-video-dude-perfect.mp3')
        }
      ],
      player: new Audio()
    }
  },
  methods: {
    play (song) {
      if (typeof song.src != "undefined") {
        this.current = song;
        this.player.src = this.current.src;
      }
      this.player.play();
      this.player.addEventListener('ended', function () {
        this.index++;
        if (this.index > this.songs.length - 1) {
          this.index = 0;
        }
        this.current = this.songs[this.index];
        this.play(this.current);
      }.bind(this));
      this.isPlaying = true;
    },
    pause () {
      this.player.pause();
      this.isPlaying = false;
    },
    next () {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    prev () {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    }
  },
  created () {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  }
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
	background-color: #333;
	color: #FFF;
}

header span {
  background-image: linear-gradient(to right, #42275a, #734b6d);
  text-transform: uppercase;
  padding: 5px;
  margin: 5px;
  border-radius: 10px;
}

main {
  width: 100%;
  max-width: 520px;
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
  font-size: 28px;
}

.controls {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 30px 0;
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
  margin: 0px;
  border-radius: 8px;
  color: #FFF;
  background-image: linear-gradient(to right, #42275a, #734b6d);
}

.next, .prev {
  font-size: 16px;
  font-weight: 700;
  padding: 10px 20px;
  margin: 0px;
  border-radius: 6px;
  color: #FFF;
  background-image: linear-gradient(to right, #42275a, #734b6d);
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

.playlist .song-artist {
  font-weight: 300;
  font-size: 14px;
}

.playlist .song:hover {
  color: #c0c0c0;
}

.playlist .song.playing {
  color: #FFF;
  background-image: linear-gradient(to right, #42275a, #734b6d);
}
</style>