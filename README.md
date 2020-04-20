# lottie-web-vue

> a vue component for lottie-web

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

## Component use

Install through npm:

```
npm install --save lottie-web-vue
```

```
# use component
<template>
  <div id="app">
    <p class="btn">
      <span @click="lottiePlay">开始</span>
      <span @click="lottieRePlay">重置</span>
      <span @click="lottiePause">暂停</span>
      <span @click="lottieStop">停止</span>
    </p>
    <div class="lottie-wrap">
      <Lottie
        :width="400"
        :height="400"
        :loop="loop"
        :animation-data="pickData"
        @getLottieInstance="getLottieInstance"
        @onComplete="onComplete"
      />
    </div>
    <p>
      速度：<input class="speed" type="range" value="1" min="0" max="3" step="0.5" @change="onSpeedChange" v-model="animationSpeed">
    </p>
  </div>
</template>

<script>
import Lottie from './components/lottie'
import pickData from './assets/lottieJson/pick.json'
import DissData from './assets/lottieJson/diss.json'
export default {
  name: 'App',
  components: {
    Lottie
  },
  data () {
    return {
      pickData,
      DissData,
      lottieInstance: '',
      loop: true,
      animationSpeed: 1
    }
  },

  methods: {
    getLottieInstance (lottieInstance) {
      this.lottieInstance = lottieInstance
    },
    lottiePlay () {
      this.lottieInstance && this.lottieInstance.play()
    },
    lottieRePlay () {
      this.animationSpeed = 1
      this.lottieInstance && this.lottieInstance.stop()
      setTimeout(() => {
        this.lottieInstance.play()
        this.onSpeedChange()
      })
    },
    lottiePause () {
      this.lottieInstance && this.lottieInstance.pause()
    },
    lottieStop () {
      this.lottieInstance && this.lottieInstance.stop()
    },
    onComplete (e) {
      console.log('onComplete', e)
    },
    onSpeedChange: function () {
      this.lottieInstance.setSpeed(this.animationSpeed)
    }
  }
}
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.btn span {
  display: inline-block;
  margin: 20px 30px;
  font-weight: bold;
  cursor: pointer;
}
.speed {
  cursor: pointer;
}
.lottie-wrap {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>

```