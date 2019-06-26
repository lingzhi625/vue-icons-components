<template>
  <div class="circleCanvas" style="display:inline-block">
    <canvas class="myCanvas" :id="canvasKey"
      >Your browser does not support the HTML5 canvas tag.</canvas
    >
  </div>
</template>
<script>
export default {
  // 使用说明：percentVal-百分比值，color-显示的颜色，size-icon的展示size，单位（px），canvasKey-canvas的key值，唯一！
  props: {
    percentVal: String,
    color: String,
    size: {
      type: String,
      default: '30'
    },
    canvasKey: String
  },
  data () {
    return {
      start: 0,
      end: 0,
      nums: [0.9, 0.1], // 第一个元素代表第几期，第二个参数代表剩余的期数
      colors: ['#f00', '#fff']
    }
  },
  methods: {
    createCanvas () {
      this.initParams(this.percentVal, this.color)
      var canvas = {}
      this.$nextTick(() => {
        canvas = document.getElementById(this.canvasKey)
        canvas.style.width = this.size + 'px'
        canvas.style.height = this.size + 'px'
        canvas.height = this.size * 2
        canvas.width = this.size * 2
        var ctx = canvas.getContext('2d')
        ctx.translate(this.size * 1, this.size * 1)
        this.pieChart(ctx)
      })
    },
    initParams (num, color) {
      this.colors[0] = color
      if (!isNaN(num * 1) && num * 1 >= 0 && num <= 1) {
        this.nums[0] = num * 1
        this.nums[1] = 1 - num
      } else if (num.indexOf('/') !== -1) {
        var numLs = num.split('/')
        this.nums[0] = (numLs[0] / numLs[1]) * 1
        this.nums[1] = ((numLs[1] - numLs[0]) / numLs[1]) * 1
      } else {
        alert('请输入小数或者分数格式')
      }
    },
    pieChart (ctx) {
      ctx.rotate((-90 * Math.PI) / 180)
      for (var i = 0; i < this.nums.length; i++) {
        ctx.beginPath()
        ctx.moveTo(0, 0)
        this.end += (this.nums[i] / 0.5) * Math.PI // 终止角度
        ctx.strokeStyle = this.color
        ctx.fillStyle = this.colors[i]
        ctx.arc(0, 0, this.size - 2, this.start, this.end)
        ctx.fill()
        ctx.closePath()
        ctx.stroke()
        this.start += (this.nums[i] / 0.5) * Math.PI // 起始角度
      }
    }
  },
  created () {
    this.createCanvas()
  }
}
</script>
<style scope>
/* .circleCanvas {
  float: left;
} */
</style>
