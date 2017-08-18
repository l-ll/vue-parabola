<template>
  <div class="parabola-ball" v-show="isTrigger" :style="{transform: 'translate(' + parabola.translateX + 'px, ' + parabola.translateY + 'px)'}"></div>
</template>

<script>
  export default {
    data () {
      return {
        isAnimating: false,
        parabolas: [],
        parabola: {}
      }
    },
    props: {
      start: Object,
      end: Object,
      isTrigger: Boolean,
      onParabolaFinish: Function,
      top: {
        type: Number,
        default: 1
      },
      duration: {
        type: Number,
        default: 500
      },
      rate: {
        type: Number,
        default: 1
      }
    },
    watch: {
      isTrigger (val) {
        if (val) {
          let parabola = {
            start: this.start,
            end: this.end,
            translateX: 0,
            translateY: 0,
            startTime: Date.now(),
            animationEnd: false
          }
          this.addBall(parabola)
        }
      }
    },
    methods: {
      addBall (parabola) {
        this.parabolas.push(parabola)
        if (!this.isAnimating) {
          this.updateBalls()
        }
      },
      updateBalls () {
        this.isAnimating = true
        if (this.parabolas.length === 0) {
          this.isAnimating = false
          this.$emit('parabolafinish')
          this.onParabolaFinish && this.onParabolaFinish()
          return
        }
        let {duration, rate, top: rry1} = this
        let parabolas = this.parabolas.map(parabola => {
          let {start, end, startTime, animationEnd} = parabola
          let interval = Date.now() - startTime
          if (interval > duration) {
            if (animationEnd) {
              return null
            } else {
              interval = duration
              parabola.animationEnd = true
            }
          }
          let percent = interval / duration
          let {x: rx1, y: ry1} = start
          let {x: rx2, y: ry2} = end
          let direction = rx2 > rx1 ? 1 : -1
          let lmy1 = ry2 - rry1
          let my1 = ry2 - ry1
          let mx2 = direction * (rx2 - rx1)

          let lmh = mx2 / 2
          let mh = (1 - my1 / lmy1) * lmh * (rate) * (1 - my1 / lmy1) + lmh * (1 - rate) * (1 + my1 / lmy1)
          let a = my1 / mx2 / (2 * mh - mx2)
          let b = -2 * a * mh
          let c = my1
          let mx = percent * mx2
          let my = a * Math.pow(mx, 2) + b * mx + c
          parabola.translateX = rx1 + direction * mx
          parabola.translateY = ry2 - my
          return parabola
        })
        parabolas = parabolas.filter((parabola) => {
          return parabola != null
        })
        this.parabolas = parabolas
        if (this.parabolas.length) {
          this.parabola = parabolas[0]
        }

        requestAnimationFrame(this.updateBalls.bind(this))
      }
    }
  }
</script>

<style>
  .parabola-ball {
    position: absolute;
    left: 0;
    top: 0;
    width: 40px;
    height: 40px;
    border-radius: 20px;
    background-color: #f1c643;
    z-index: 300;
  }
</style>
