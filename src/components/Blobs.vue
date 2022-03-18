<script setup lang="ts">
import { computed, onMounted, reactive, ref } from 'vue'

const el = ref<HTMLCanvasElement>()
const colorPallete = ["#ff1783", "#17c9ff", "#36ff40"]

interface Position {
  x: number
  y: number
}

const width = ref(window.innerWidth)
const height = ref(window.innerHeight)
const mouse = ref<Position>({x: width.value / 2, y: width.value / 2})
const origin = computed<Position>(() => { return { x: width.value / 2, y: height.value / 2} })
const balls = reactive([])
const count = ref(0)
const randomCount = ref(0)

const rConst = 40
const r = ref(rConst)
const mousedown = ref(false)
const mousemove = ref(false)

const drawOriginBall = () => {
  const { ctx: context } = getCanvas()
  context.fillStyle = "#ffdd02";
  context.beginPath();
  context.arc(origin.value.x, origin.value.y, r.value, 0, Math.PI * 2, false);
  context.fill();
  if (mousedown.value) {
    r.value += 0.5
  } else {
    r.value -= 1
    if (r.value < rConst) {
      r.value = rConst
    }
  }
}

function getCanvas() {
  const canvas = el.value!
  const ctx = el.value!.getContext('2d')!
  return { canvas, ctx }
}

window.onresize = () => {
  const { canvas } = getCanvas()
  width.value = canvas.width = window.innerWidth
  height.value = canvas.height = window.innerHeight
}

class Ball{
  x: number
  y: number
  angle: number
  vx: number
  vy: number
  r: number
  color: string
  constructor(){
    this.x = origin.value.x;
    this.y = origin.value.y;
    this.angle = Math.PI * 2 * Math.random();
    this.vx = (1.3 + Math.random() * .3) * Math.cos(this.angle);
    this.vy = (1.3 + Math.random() * .3) * Math.sin(this.angle);
    this.r = 6 + 3 * Math.random();
    while (this.r <= 0) { this.r = 6 + 3 * Math.random(); }
    this.color = colorPallete[Math.floor(Math.random() * colorPallete.length)];
  }
  
  update(){
    this.x += this.vx;
    this.y += this.vy;
    this.r -= .01;
    if (this.r <= 0) { this.r = 0; }
  }
}

onMounted(() => {
  const { canvas, ctx: context } = getCanvas()
  canvas.width = width.value
  canvas.height = height.value
  function loop() {
    context.clearRect(0, 0, width.value, height.value);
    if(count.value === randomCount.value){
      balls.push(new Ball());
      count.value = 0;
      randomCount.value = 3 + Math.floor(Math.random() * 25);
    }
    count.value++;
    if (mousemove.value) {
      if (Math.random() < 0.3) {
        balls.push(new Ball());
      }
    }
    for(var i = 0; i < balls.length; i++){
      var b = balls[i];
      context.fillStyle = b.color;
      context.beginPath();
      context.arc(b.x, b.y, b.r, 0, Math.PI * 2, false);
      context.fill();
      b.update();
    }

    origin.value.x += (mouse.value.x - origin.value.x) * .15;
    origin.value.y += (mouse.value.y - origin.value.y) * .15;

    drawOriginBall();

    removeBall();
    requestAnimationFrame(loop);
  }
  loop();
})

function removeBall(){
  for(var i = 0; i < balls.length; i++){
    var b = balls[i];
    if(
      b.x + b.r < 0 ||
      b.x - b.r > width.value ||
      b.y + b.r < 0 ||
      b.y - b.r > height.value ||
      b.r <= 0
    ) {
      balls.splice(i, 1);
    }
  }
}

window.addEventListener("mousemove", function(e) {
  mouse.value.x = e.clientX;
  mouse.value.y = e.clientY;
}, false);

window.addEventListener("mousedown", function() {
  mousedown.value = true
}, false)

window.addEventListener("mouseup", function() {
  mousedown.value = false
}, false)

window.addEventListener("mousemove", function() {
  mousemove.value = true
}, false)

setInterval(() => {
  mousemove.value = false
}, 500)
</script>

<template>
  <canvas id="canvas" ref="el"></canvas>
  <svg xmlns="http://www.w3.org/2000/svg" version="1.1">
    <defs>
      <filter id="goo">
      <feGaussianBlur in="SourceGraphic" stdDeviation="10" result="blur" />
      <feColorMatrix in="blur" mode="matrix" values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 60 -9"/>
      </filter>
    </defs>
  </svg>
</template>

<style>
* {
  cursor: none;
}

body {
  background: #123;
}

#canvas{
  position: absolute;
  -webkit-filter: url("#goo");
  filter: url("#goo");
  position: fixed;
  left: 0; right: 0; top: 0; bottom: 0;
}

#canvas {
  z-index: -100;
}
</style>
