<template>
  <el-container>
    <Left></Left>
    <el-container>
      <el-header>
        <Header></Header>
      </el-header>
      <el-main>
        <Back></Back>
      </el-main>
      <!--<el-footer>
        <Bottom></Bottom>
      </el-footer>-->
    </el-container>
    <Right></Right>
  </el-container>

</template>

<script setup>
// import Bottom from '../bottom/landBottom.vue'
import Back from '../background/background.vue'
import Header from "../header/headerBox.vue"
import Left from './left/left.vue'
import Right from './right/right.vue'

</script>

<style lang="scss" scoped>
.el-header {
  height: 16vh;
  display: flex;
  justify-content: center;
  align-items: center;
}
.el-footer {
  height: 7.5vh;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
