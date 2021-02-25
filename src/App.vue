<template>
  <div id="app">
    <header>
      <h1 class="text text-center">MUCZ</h1>
    </header>
    <main>
      <section class="player">
        <h2 class="song-title">
          {{current.title}} -
          <span>{{current.artist}}</span>
        </h2>
        <div class="interections">
          <button class="btn btn-primary" @click="previous">Prev</button>

          <button class="btn btn-success" v-if="!isPlaying" @click="play">Play</button>

          <button class="btn btn-danger" v-else @click="pause">Pause</button>

          <button class="btn btn-primary" @click="next">Next</button>
        </div>
      </section>
      <section class="playlist">
        <h4>Your Playlist</h4>
        <button class="border border-white"
          v-for="song in songs"
          :key="song.src"
          @click="play(song)"
          :class="(song.src == current.src) ? 'song playing' : 'song'"
        >{{song.title}} - {{song.artist}}</button>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: "Good Goodbye",
          artist: "Linkin Park",
          src: require("./assets/Good Goodbye.mp3"),
        },

        {
          title: "Leave Out All the Rest",
          artist: "Linkin Park",
          src: require("./assets/Leave Out All The Rest.mp3"),
        },

        {
          title: "Tomake",
          artist: "Artcell",
          src: require("./assets/Tomake.mp3"),
        },
      ],
      player: new Audio(),
    };
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

    pause() {
      this.player.pause();
      this.isPlaying = false;
    },

    previous() {
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
  },

  created() {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
    //this.player.play();
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
  font-family: Helvetica;
}

header {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  background-color: #71de51;
  color: black;
}

main{
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 25px;
}

.song-title{
  color: #91D27E;
  font-size: 32px;
  font-weight: 800;
  text-transform: uppercase;
  text-align: center;
}

.song-title span{
  font-weight: 400;
  font-style: oblique;
}

.interections{
  display: flex;
  justify-content: center;
  align-content: center;
  padding: 30px 15px;
  align-items: center;
}

.interections button{
  padding: 10px 20px;
  margin: 0px 15px;
  border-radius: 8px;
}

.playlist {
  padding: 0px 30px;
}

.playlist h3{
  color: #71de51;
}

.playlist .song{
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 20px;
  font-weight: 700;
  cursor: pointer;
}

.playlist .song:hover{
  color: #268709;
}

.playlist .song.playing{
  color: bisque;
  background-image: linear-gradient(to right, #268709, #71de51);
  
}



</style>
