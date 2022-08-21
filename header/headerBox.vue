<template>
  <img src="@/assets/cockpit/public/标题框架.png"
       alt=""
       class="header-box">
  <img src="@/assets/cockpit/public/驾驶舱标题.png"
       alt=""
       class="header-text">
</template>

<style lang="scss" scoped>
img {
  // 解决图片失真
  image-rendering: -moz-crisp-edges; /* Firefox */
  image-rendering: -o-crisp-edges; /* Opera */
  image-rendering: -webkit-optimize-contrast; /*Webkit (non-standard naming) */
  image-rendering: crisp-edges;
  -ms-interpolation-mode: nearest-neighbor; /* IE (non-standard property) */
}
.header-box {
  width: 58vw;
  height: 16vh;
  margin-left: -17vw;
  margin-top: 0.5vh;
}
.header-text {
  width: 25vw;
  height: 4vh;
  margin-top: -1vh;
  margin-left: -42vw;
}
</style>

