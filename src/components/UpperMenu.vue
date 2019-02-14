<template>
  <div class="uppermenu">
    <div>
      <div style="background-color: rgba(255,255,255,0.8);">
        <v-icon large>menu</v-icon>
      </div>
    </div>
    <div class="box">
      <v-icon>near_me</v-icon>
      <div class="line"/>
      <div class="hash">#</div>
    </div>
  </div>
</template>
<script>
export default {};
</script>
<style scoped>
.uppermenu {
  position: absolute;
  z-index: 1;
  display: flex;
  width: 100%;
  justify-content: space-between;
  padding: 20px 20px;
}
.box {
  border: 1px solid black;
  text-align: center;
  background-color: rgba(255, 255, 255, 0.8);
}
.line {
  border-top: 2px solid black;
  margin: 0 4px;
}
.hash {
  font-size: 20px;
}
</style>
