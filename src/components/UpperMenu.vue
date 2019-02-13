<template>
  <div class="uppermenu">
    <v-icon large="">menu</v-icon>
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
}
.line {
  border-top: 2px solid black;
  margin: 0 4px;
}
.hash {
  font-size: 20px;
}
</style>
