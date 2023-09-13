
<template>
  <p class="title">幸福一桶金</p>
  <div class="box-roulette">
    <div class="markers"></div>
    <button type="button" id="btn-start" @click="rotation">
      Start
    </button>
    <div :class="['roulette', { active: isActive }]" id="roulette">
      <div class="item" v-for="(item, index) in items" :key="index" :style="{
        ...itemCommonStyle,
        'border-top-color': item.color,
        transform: `rotate(${index * d + options.angleOffset}deg)`
      }">
        <p class="label" :style="labelCommonStyle">
          <span class="text"> {{ item.prize }} </span>
          <span class="text"> {{ item.cup }} </span>
        </p>
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
import { computed, ref } from 'vue';


const options = {
  angle: 0,
  angleOffset: -36,
  speed: 5000,
  easing: "easeInOutElastic",
}


const items = [
  {
    color: '#dc0936',
    prize: '1000',
    cup: '特大杯'
  },
  {
    color: '#e6471d',
    prize: '500',
    cup: '大杯'
  },

  {
    color: '#f7a416',
    prize: '200',
    cup: '中杯'
  },
  {
    color: '#209b6c',
    prize: '-500',
    cup: '我喝一杯'
  },
  {
    color: '#60b236',
    prize: '100',
    cup: '小杯'
  },
]


const d = 360 / items.length;
const tanDeg = (deg: number) => {
  var rad = deg * Math.PI / 180;
  return wrapW / (1 / Math.tan(rad));
}

const wrapW = 500
const borderTopWidth = wrapW
const borderRightWidth = tanDeg(d);
const textH = ((2 * Math.PI * wrapW) / d) * .5;

const itemCommonStyle = {
  left: `${wrapW / 2}px`,
  top: `${-wrapW / 2}px`,
  'border-top-width': `${borderTopWidth}px`,
  'border-right-width': `${borderRightWidth}px`,

}

const labelCommonStyle = {
  height: `${textH}px`,
  'line-height': `${textH}px`,
  "transform": `translateX(${textH * 5.8}px) translateY(${wrapW * -.3}px) rotateZ(${90 + d * .48}deg)`
}

const r = (min: number, max: number) => {
  return Math.floor(Math.random() * (max - min + 1)) + min;
}

const currentAngle = ref(0)
const currentAngleStr = computed(() => `${currentAngle.value}deg`)
const targetAngle = ref(0)
const targetAngleStr = computed(() => `${targetAngle.value}deg`)

const isActive = ref(false)
const executeCount = ref(0)

const offsetMap = [1, 5, -6, 8, -4]

const rotation = () => {
  isActive.value = true
  targetAngle.value = 360 * r(5, 10) + 360 + offsetMap[executeCount.value % 5]//r(0, 360);

  currentAngle.value = parseInt((targetAngle.value % 360).toFixed(0))
  setTimeout(() => {
    isActive.value = false
  }, 2500)

  executeCount.value = executeCount.value + 1
}
</script>

<style scoped>
html,
body,
button {
  font-family: sans-serif;
}

button {
  border: 0;
  margin: 0;
  padding: 0;
}

.title {
  margin-top: 50px;
  text-align: center;
  font-size: 3rem;
  font-weight: bold;
}

.box-roulette {
  position: relative;
  margin: 50px auto;
  width: 500px;
  height: 500px;
  border: 10px solid #ccc;
  border-radius: 50%;
  background: #ccc;
  overflow: hidden;
}

.box-roulette .markers {
  position: absolute;
  left: 50%;
  top: 0;
  margin-left: -25px;
  width: 0;
  height: 0;
  border: 25px solid #fff;
  border-left-color: transparent;
  border-right-color: transparent;
  border-bottom-color: transparent;
  z-index: 9999;
}

.box-roulette .roulette {
  position: relative;
  width: 100%;
  height: 100%;
  overflow: hidden;
}

.box-roulette .item {
  position: absolute;
  top: 0;
  width: 0;
  height: 0;
  border: 0 solid transparent;
  transform-origin: 0 100%;
}

.box-roulette .label {
  position: absolute;
  left: 0;
  top: 0;
  color: #fff;
  white-space: nowrap;
  transform-origin: 0 0;
  display: flex;
  flex-direction: column;
}

.box-roulette .label .text {
  display: inline-block;
  font-size: 20px;
  font-weight: bold;
  line-height: 1;
  vertical-align: middle;
  text-align: center;
}

#btn-start {
  display: block;
  position: absolute;
  left: 50%;
  top: 50%;
  margin: -50px 0 0 -50px;
  width: 100px;
  height: 100px;
  font-weight: bold;
  background: #fff;
  border-radius: 50px;
  z-index: 9999;
  cursor: pointer;
}

#roulette {

  transform: rotate(v-bind('currentAngleStr'));

  &.active {
    transition: transform 2s ease-in-out;
    transform: rotate(v-bind('targetAngleStr'));
  }
}
</style>
