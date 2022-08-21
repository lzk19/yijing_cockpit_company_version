<template>
  <div>
    <img src="@/assets/cockpit/public/背景图层.png"
         class="back"
         alt="">
    <img src="@/assets/cockpit/public/两侧装饰.png"
         alt=""
         class="around">

  </div>
</template>

<script>

</script>
<style lang="scss" scoped>
.back {
  position: absolute;
  height: 100%;
  width: 100%;
  left: 0vw;
  top: 0vh;
  z-index: -1;
}

.around {
  position: absolute;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  z-index: -2;
  /*box-shadow: 0vw 8vh 20vw 10vw #061648 inset;*/
  box-shadow: inset 0px 120px 160px 0px #061648,
  inset 320px 0px 320px 0px #061648,
  inset -320px 0px 360px 0px #061648,
  inset 0px -121px 150px 0px #061648;
}
img {
  // 解决图片失真
  image-rendering: -moz-crisp-edges; /* Firefox */
  image-rendering: -o-crisp-edges; /* Opera */
  image-rendering: -webkit-optimize-contrast; /*Webkit (non-standard naming) */
  image-rendering: crisp-edges;
  -ms-interpolation-mode: nearest-neighbor; /* IE (non-standard property) */
}
</style>
