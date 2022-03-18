<template>
  <div class="bottom">
    <a href="https://beian.miit.gov.cn"> 津ICP备2022002066号-1 </a>
    <a href="http://www.beian.gov.cn/portal/registerSystemInfo"> 津公网安备 </a>
    <a href="https://codepen.io/mnmxmx/pen/VjjvEq"> Blobs Idea From codepen.io/mnmxmx </a>
  </div>
</template>

<style>
.bottom {
  font-size: .05rem;
  display: flex;
  width: 100vw;
  position: absolute;
  margin-left: auto; margin-right: auto;
  bottom: .05rem;
  justify-content: center;
  align-items: center;
}

.bottom > a {
  color: rgb(122, 122, 122);
  text-decoration: none;
  display: block;
  padding: .05rem;
  text-align: center;
}

@keyframes color-hover {
  from { color: rgb(122, 122, 122); }
  to { color: rgb(255, 0, 0) }
}

.bottom > a:hover {
  animation: color-hover alternate-reverse infinite 2s ease-in-out;
}

.bottom > a+a::before {
  content: '|';
  margin-left: -.075rem;
}

.bottom > a+a::before:hover {
  /* 不知如何禁止伪元素动画 */
  animation: none;
}

@media only screen and (max-width: 600px) {
  .bottom {
    flex-direction: column;
  }

  .bottom > a+a::before {
    content: '';
  }
}
</style>