<template>
  <div class="lottie" :style="style" ref="lottie"></div>
</template>

<script>
import lottie from 'lottie-web'
export default {
  props: {
    height: Number,
    width: Number,
    renderer: {
      type: String,
      default: 'svg'
    },
    loop: {
      type: Boolean,
      default: true
    },
    autoplay: {
      type: Boolean,
      default: true
    },
    animationData: {
      type: Object,
      required: true
    }
  },
  data () {
    return {
      instance: false,
      style: {
        width: this.width ? `${this.width}px` : '100%',
        height: this.height ? `${this.height}px` : '100%',
        overflow: 'hidden',
        margin: '0 auto'
      }
    }
  },
  mounted () {
    // 获取lottie参数 生成对象 并且扩展方法
    const { renderer, loop, autoplay, animationData } = this
    const paramsInfo = {
      container: this.$refs.lottie,
      renderer,
      autoplay,
      loop,
      animationData
    }
    // lottie实例生成
    this.instance = lottie.loadAnimation(paramsInfo)
    this.transEvents(this.instance)
    // 抛出getLottieInstance事件 父节点获取动画对象
    this.$emit('getLottieInstance', this.instance)
  },
  methods: {
    // 透传注册事件
    transEvents (instance) {
      // complete: 播放完成（循环播放下不会触发）
      // loopComplete: 当前循环下播放（循环播放/非循环播放）结束时触发
      // enterFrame: 每进入一帧就会触发，播放时每一帧都会触发一次，stop方法也会触发
      // segmentStart: 播放指定片段时触发，playSegments、resetSegments等方法刚开始播放指定片段时会发出，如果playSegments播放多个片段，多个片段最开始都会触发。
      const events = ['onComplete', 'onLoopComplete', 'onEnterFrame', 'onSegmentStart']
      events.forEach(keyName => {
        instance[keyName] = e => this.$emit(keyName, e)
      })
    }
  }
}
</script>

<style scope>
.lottie {
  font-size: 0;
}
</style>
