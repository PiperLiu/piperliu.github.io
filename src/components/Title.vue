<script setup lang="ts">
const props = defineProps<{
  msg: string
}>()
</script>

<template>
  <div class="title">
    <h1 :data-spotlight="props.msg">{{ props.msg }}</h1>
  </div>
</template>

<style>
.title {
  margin-top: 1.6rem;
  font-size: .8rem;
  font-weight: bolder;
  text-align: center;
  font-family: Arial, Helvetica, sans-serif;
}

.title > h1 {
  color: #123;
  font-family: Helvetica;
  margin: 0;
  padding: 0;
  font-size: .75rem;
  position: relative;
}

.title > h1::after {
  content: attr(data-spotlight);
  color: transparent;
  position: absolute;
  top: 0; left: 0; right: 0; bottom: 0;
  -webkit-clip-path: ellipse(1rem 1rem at 0% 50%);
  clip-path: ellipse(1rem 1rem at 0% 50%);
  animation: spotlight 10s infinite;
  background: linear-gradient(135deg, rgb(243, 9, 192) 30%, cyan);
  background-size: 150%;
  background-position: center center;
  -webkit-background-clip: text;
  background-clip: text;
}

@keyframes spotlight {
  0% {
    -webkit-clip-path: ellipse(1rem 1rem at 0% 50%);
    clip-path: ellipse(1rem 1rem at 0% 50%);
  }
  
  50% {
    -webkit-clip-path: ellipse(1rem 1rem at 100% 50%);
    clip-path: ellipse(1rem 1rem at 100% 50%);
  }
  
  100% {
    -webkit-clip-path: ellipse(1rem 1rem at 0% 50%);
    clip-path: ellipse(1rem 1rem at 0% 50%);
  }
}

@media only screen and (max-width: 600px) {
  @keyframes spotlight {
    0% {
      -webkit-clip-path: ellipse(2rem 2rem at -50% 50%);
      clip-path: ellipse(2rem 2rem at -50% 50%);
    }
    
    50% {
      -webkit-clip-path: ellipse(2rem 2rem at 150% 50%);
      clip-path: ellipse(2rem 2rem at 150% 50%);
    }
    
    100% {
      -webkit-clip-path: ellipse(2rem 2rem at -50% 50%);
      clip-path: ellipse(2rem 2rem at -50% 50%);
    }
  }
}

@media only screen and (max-width: 600px) {
  .title {
    margin-top: 1rem;
  }
}
</style>