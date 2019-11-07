<template>
  <div id="app" :class="{ in: fadeInApp }">
    <div class="countdown-wrap" :class="{ in: fadeInCount }">
      <span class="time">{{ timer }}</span>
    </div>
    <div class="copy">
      This is a fan site, all rights to Death Stranding belong fully to &copy;
      <a href="http://kojimaproductions.jp">Kojima Productions</a>
    </div>
  </div>
</template>

<script>
import { DateTime } from "luxon"

export default {
  name: "app",
  data() {
    return {
      fadeInApp: false,
      fadeInCount: false,
      days: "0",
      hours: "00",
      minutes: "00",
      seconds: "00",
    }
  },
  created() {
    this.preload()

    setInterval(() => {
      this.tick()
    }, 1000)
  },
  computed: {
    timer() {
      const { days, hours, minutes, seconds } = this
      return `${days}d ${hours}h ${minutes}m ${seconds}s`
    },
  },
  methods: {
    tick() {
      const release = DateTime.fromISO("2019-11-08")
      const duration = release.diffNow(["days", "hours", "minutes", "seconds"])

      this.days = `${duration.days}`
      this.hours = `${duration.hours}`.padStart(2, "0")
      this.minutes = `${duration.minutes}`.padStart(2, "0")
      this.seconds = `${Math.floor(duration.seconds)}`.padStart(2, "0")
    },
    preload() {
      const img = new Image()
      img.src = require("./assets/bg_1.jpg")
      img.onload = () => {
        console.log(img.src)
        console.log("loaded!")
        this.beginFadeIn()
      }
    },
    beginFadeIn() {
      setTimeout(() => {
        this.fadeInApp = true
      }, 500)

      setTimeout(() => {
        this.fadeInCount = true
      }, 2000)
    },
  },
}
</script>

<style scoped>
#app {
  height: 100vh;
  width: 100vw;
  position: relative;
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  background-image: url("./assets/bg_1.jpg");
  background-position: center;
  background-size: cover;
  background-repeat: no-repeat;
  transition: opacity 1.5s ease;
  opacity: 0;
}
#app.in {
  opacity: 1;
}
.countdown-wrap {
  position: absolute;
  bottom: 30%;
  width: 100%;
  transition: all 1.5s ease;
  opacity: 0;
  transform: translateY(-20px);
}
.countdown-wrap.in {
  opacity: 1;
  transform: translateY(0);
}
.time {
  font-size: 3.5rem;
  color: #121619;
  text-shadow: white 0px 0px 12px;
  background: rgba(255, 255, 255, 0.5);
  padding: 10px 30px;
  border-radius: 5px;
}
.copy {
  position: absolute;
  bottom: 0;
  width: 100%;
  color: #ccc;
  font-size: 0.8rem;
}
.copy > a {
  color: #ccc;
  text-decoration: none;
}

@media screen and (max-width: 767px) {
  #app {
    background-size: 230vw;
  }

  .time {
    font-size: 2.5rem;
  }
}
</style>
