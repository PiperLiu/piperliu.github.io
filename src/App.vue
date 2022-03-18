<script setup>
import Blobs from './components/Blobs.vue'
import Bottom from './components/Bottom.vue'
import Title from './components/Title.vue'
import Infos from './components/Infos.vue'
import { ref } from 'vue';

const infosObjs = {
  '代码 | GitHub': () => { console.log('github') },
  '关于 | About': () => { console.log('about') },
  '联系 | Contact': () => { console.log('github') }
}

const quesTime = 2000
const showQuesTime = ref(quesTime)
const mouseOverTitle = () => { showQuesTime.value = quesTime + 10 }
const mouseOutTitle = () => { showQuesTime.value = quesTime }
setInterval(() => {
  if (showQuesTime.value <= quesTime) {
    showQuesTime.value -= 100
  } else if (showQuesTime.value <= 0) {
    showQuesTime.value = 0
  }
}, 100)
</script>

<template>
  <div class="ques" :style="{ opacity: showQuesTime / quesTime }" v-if="showQuesTime >= 0">
    <scan>
      What is that? 这是什么？
    </scan>
  </div>
  <Title msg="Good Luck Sampling"
    @mouseover="mouseOverTitle"
    @mouseout="mouseOutTitle"
  ></Title>
  <div class="name">
    <scan>刘洪佳 | Piper Liu</scan>
  </div>
  <Infos :objs="infosObjs"></Infos>
  <Bottom></Bottom>
  <Blobs></Blobs>
</template>

<style>
.ques {
  position: absolute;
  width: 100vw;
  text-align: center;
  top: .3rem;
  font-size: .2rem;
  font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
  color: azure;
  letter-spacing: .04rem;
}

.name {
  color: azure;
  font-size: .2rem;
  text-align: center;
  margin-top: .3rem;
}

.name > scan {
  font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
  padding-left: .2rem; padding-right: .2rem;
  letter-spacing: .05rem;
}

.name > scan:hover {
  color: rgb(142, 17, 245);
  transition: all .3s ease-in-out;
}

@media only screen and (max-width: 600px) {
  .name {
    margin-top: .4rem;
  }
}
</style>
