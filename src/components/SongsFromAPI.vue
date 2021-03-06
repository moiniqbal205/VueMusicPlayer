<template>
  <p>Work In Progress</p>
  <!-- <div id="app">
    <header>
      <h1>devTunes</h1>
    </header>
    <main>
      <section class="player">
        <h2 class="song-title">{{ current.name }} - <span>{{ current.artistName }}</span></h2>
        <div class="controls">
          <button class="prev" @click="prev">Prev</button>
          <button class="play" v-if="!isPlaying" @click="play">Play</button>
          <button class="pause" v-else @click="pause">Pause</button>
          <button class="Next" @click="next">Next</button>
        </div>
      </section>
      <section class="playlist">
        <h3>Playlist</h3>
        <button 
          v-for="(song, index) in songs" 
          :key="index" 
          @click="play(song)" 
          :class="(song.previewURL == current.previewURL ? 'song playing' : 'song')"
        >
          {{ song.name }} - {{ song.artistName }}
        </button>
        <button 
          v-for="(song, index) in songs" 
          :key="index"
          @click="song.previewURL"
          :class="(song == current ? 'song playing' : 'song')"
        >
          {{ song.name }} - {{ song.artistName }}
        </button><br/>
      </section>
    </main>
  </div> -->
</template>

<script>
import axios from 'axios';

export default {
  name: 'App',
  components: {},
  data () {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [],
      player: new Audio()
    }
  },
  methods: {
    play (song) {
      if (typeof song.previewURL != "undefined") {
        for(var i=0;i<this.songs.length;i++) {
          this.current = this.songs[i];
          this.player = this.current.previewURL;
        }
      }

      this.player.play();
      this.player.addEventListener('ended', this.next().bind(this));
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
    this.player = this.current;
  },
  mounted () {
    // from https://developer.napster.com/examples
    axios.get('https://api.napster.com/v2.1/tracks/top?apikey=ZTk2YjY4MjMtMDAzYy00MTg4LWE2MjYtZDIzNjJmMmM0YTdm')
    .then(response => (this.songs = response.data.tracks));
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

header {
  display: block;
  text-align: center;
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
  padding: 30px 15px;
  align-items: center;
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
  margin: 0 15px;
  border-radius: 8px;
  color: #FFF;
  background-color: #CC2E5D;
}

.next, .prev {
  font-size: 16px;
  font-weight: 700;
  padding: 10px 20px;
  margin: 0 15px;
  border-radius: 6px;
  color: #FFF;
  background-color: #FF5858;
}

.playlist {
  padding: 0 30px;
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
