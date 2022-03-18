<script setup lang="ts">
const props = defineProps<{
  objs: Record<string, Function>
}>()
</script>

<template>
  <div class="header">
    <scan
      v-for="v, k in props.objs"
      :key="k"
      @click="v()"
    >{{ k }}</scan>
  </div>
</template>

<style>
.header {
  font-size: .3rem;
  padding: 0;
  margin: .5rem;
  display: flex;
  flex: 1;
  flex-direction: column;
  text-align: center;
  align-items: center;
  justify-content: space-evenly;
}

.header > scan {
  font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
  color: rgb(108, 122, 134);
  padding: .2rem
}

@keyframes hover {
  from {
  } to {
    color: #fff;
  }
}

.header > scan:hover {
  animation: hover .3s ease-in-out;
  animation-fill-mode: forwards;
}

@keyframes active {
  from {
    color: #fff;
  } to {
    color: #fff;
    filter: blur(10px);
  }
}

.header > scan:active {
  animation: active .3s ease-in-out;
  animation-fill-mode: forwards;
}

@media only screen and (max-width: 600px) {
  .header {
    margin-top: .3rem;
  }
  .header > scan {
    font-size: .2rem;
    padding: .1rem
  }
}
</style>
