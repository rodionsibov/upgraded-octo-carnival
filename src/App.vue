<template>
  <header>
    <h1>My Music</h1>
  </header>
  <main>
    <section class="player">
      <h2 class="song-title">
        {{ current.title }} - <span>{{ current.artist }}</span>
      </h2>
      <div class="controls">
        <button class="prev" @click="prev">Prev</button>
        <button v-if="!isPlaying" class="play" @click="play">Play</button>
        <button v-else class="pause" @click="pause">Pause</button>
        <button class="next" @click="next">Next</button>
      </div>
    </section>
    <section class="playlist">
      <h3>The Playlist</h3>
      <button
        v-for="song in songs"
        :key="song.src"
        @click="play(song)"
        :class="song.src === current.src ? 'song playing' : 'song'"
      >
        {{ song.title }} - {{ song.artist }}
      </button>
    </section>
  </main>
</template>

<script>
export default {
  name: "App",
  created() {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  },
  methods: {
    play(song) {
      if (typeof song.src != "undefined") {
        this.current = song;
        this.player.src = this.current.src;
      }
      this.player.play();
      this.player.addEventListener(
        "ended",
        function () {
          this.index++;

          if (this.index > this.songs.length - 1) {
            this.index = 0;
          }

          this.current = this.songs[this.index];
          this.play(this.current);
        }.bind(this)
      );
      this.isPlaying = true;
    },
    prev() {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    next() {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    pause() {
      this.player.pause();
      this.isPlaying = false;
    },
  },
  data() {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      player: new Audio(),
      songs: [
        {
          title: "Happy Rock",
          artist: "Benjamin Tissot (also known as Bensound)",
          info:
            "An uplifting positive and happy rock royalty free music track featuring drums, bass, hand claps and electric guitar catchy riffs. Image copyright: alicedaniel - Shutterstock",
          img: "https://www.bensound.com/bensound-img/happyrock.jpg",
          src: "https://www.bensound.com/bensound-music/bensound-happyrock.mp3",
        },
        {
          title: "Jazzy Frenchy",
          artist: "Benjamin Tissot (also known as Bensound)",
          info:
            "Gypsy french jazz royalty free music track featuring acoustic guitar, double bass, accordion and vibes. Perfect for a comedy video or a project about Paris or France.",
          img: "https://www.bensound.com/bensound-img/jazzyfrenchy.jpg",
          src:
            "https://www.bensound.com/bensound-music/bensound-jazzyfrenchy.mp3",
        },
      ],
    };
  },
};
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
  color: #fff;
}

main {
  width: 100%;
  max-width: 700px;
  margin: 0 auto;
  padding: 25px;
}

.song-title {
  color: black;
  font-size: 42px;
  text-align: center;
}

.song-title span {
  font-weight: normal;

  color: gray;
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

.play,
.pause {
  font-size: 20px;
  font-weight: bold;
  padding: 15px 25px;
  border-radius: 8px;
  color: #fff;
  background-color: green;
}

.next,
.prev {
  font-size: 16px;
  font-weight: bold;
  padding: 10px 20px;
  margin: 0 10px;
  border-radius: 6px;
  color: #fff;
  background-color: gray;
}

.playlist {
  padding: 0 3px;
}

.playlist h3 {
  color: #212121;
  font-size: 28px;
  margin-bottom: 30px;
  text-align: center;
}

.playlist .song {
  display: flex;
  width: 100%;
  padding: 15px;
  font-size: 16px;
  font-weight: bold;
  cursor: pointer;
}

.playlist .song.playing {
  color: #fff;
  border-radius: 8px;
  background-image: linear-gradient(to right, green, lime);
}

.playlist .song:hover {
  opacity: 0.8;
}
</style>
