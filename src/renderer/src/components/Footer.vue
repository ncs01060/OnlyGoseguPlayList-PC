<template>
  <div class="footerDiv">
    <footer>
      <button @click="playPreviousSong" :disabled="currentSongIndex <= 0">
        <img src="../assets/img/backword.svg" />
      </button>
      <button @click="playsound()">
        <img v-if="sw" src="../assets/img/pause.svg" alt="일시정지 아이콘" />
        <img v-else src="../assets/img/play.svg" alt="재생 아이콘" />
      </button>
      <button @click="playNextSong" :disabled="currentSongIndex >= playlist.length - 1">
        <img src="../assets/img/forword.svg" />
      </button>
      <audio
        ref="audioPlayer"
        controls
        autoplay
        style="display: none"
        @ended="playNextSong"
        @timeupdate="updateCurrentTime"
        @durationchange="updateDuration"
      >
        <source :src="currentSong" type="audio/mp3" />
      </audio>
      <p>{{ formatTime(duration) }} : {{ formatTime(currentTime) }}</p>
    </footer>
  </div>
</template>
<script>
import song1 from '../assets/song/1.mp3'
import song2 from '../assets/song/2.mp3'
import song3 from '../assets/song/3.mp3'

export default {
  data() {
    return {
      playlist: [song1, song2, song3],
      currentSong: '',
      currentSongIndex: -1,
      currentTime: 0,
      duration: 0,
      sw: false,
      pauseTime: 0
    }
  },
  methods: {
    playsound: function () {
      if (this.sw == false) {
        this.currentSongIndex = 0
        this.playCurrentSong()
      } else {
        this.$refs.audioPlayer.pause()
        this.sw = false
        this.pauseTime = this.$refs.audioPlayer.currentTime
      }
    },
    playCurrentSong: function () {
      if (this.currentSongIndex >= 0 && this.currentSongIndex < this.playlist.length) {
        this.$refs.audioPlayer.src = this.playlist[this.currentSongIndex]
        this.$refs.audioPlayer.load()
        if (this.pauseTime > 0) {
          // 일시정지된 시간이 저장되어 있다면 해당 시간부터 재생합니다.
          this.$refs.audioPlayer.currentTime = this.pauseTime
          this.pauseTime = 0
        }

        this.$refs.audioPlayer.play()
        this.sw = true
      }
    },
    playNextSong: function () {
      // 다음 노래 재생
      this.currentSongIndex++
      if (this.currentSongIndex >= this.playlist.length) {
        // 플레이리스트의 끝에 도달하면 처음 노래로 돌아갑니다.
        this.currentSongIndex = 0
      }
      this.playCurrentSong()
    },
    updateCurrentTime: function () {
      // 현재 노래의 진행 시간 업데이트
      this.currentTime = this.$refs.audioPlayer.currentTime
    },
    updateDuration: function () {
      // 현재 노래의 총 재생 시간 업데이트
      this.duration = this.$refs.audioPlayer.duration
    },
    formatTime: function (seconds) {
      // 초를 분:초 형식으로 변환
      const minutes = Math.floor(seconds / 60)
      const remainingSeconds = Math.floor(seconds % 60)
      return `${minutes}:${remainingSeconds < 10 ? '0' : ''}${remainingSeconds}`
    },
    playPreviousSong: function () {
      // 이전 노래 재생
      this.currentSongIndex--
      if (this.currentSongIndex < 0) {
        // 현재 노래가 첫 번째 노래인 경우, 마지막 노래로 이동합니다.
        this.currentSongIndex = this.playlist.length - 1
      }
      this.playCurrentSong()
    },
    playNextSong: function () {
      // 다음 노래 재생
      this.currentSongIndex++
      if (this.currentSongIndex >= this.playlist.length) {
        // 플레이리스트의 끝에 도달하면 처음 노래로 돌아갑니다.
        this.currentSongIndex = 0
      }
      this.playCurrentSong()
    }
  }
}
</script>
<style></style>
