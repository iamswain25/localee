<template>
  <section :class="isSearchBoxUp ? `searchtop` : `searchbottom` ">
    <div class="line" @click="searchBoxUp"/>
    <div class="box">
      <span style="padding: 0 5px;">#</span>
      <input class="input" type="text" v-model="searchKey">
      <!-- <span v-show="searchKey.length"></span> -->
      <v-icon large v-show="searchKey.length" @click="clearSerchKey">clear</v-icon>
    </div>
    <!-- <transition name="enlarge"> -->
    <article v-show="isSearchBoxUp" class="article">
      <div>내 지역 핫토픽</div>
      <ul>
        <li>양승태</li>
        <li>남북회담</li>
        <li>손흥민</li>
        <li>방탄소년단</li>
        <li>김경수</li>
        <li>황서원</li>
        <li>이현노</li>
        <li>로컬이</li>
        <li>토픽또</li>
        <li>로고</li>
      </ul>
      <div>토픽 더 보기</div>
      <div class="button">토픽쓰기</div>
    </article>
    <!-- </transition> -->
  </section>
</template>
<script>
export default {
  data() {
    return {
      searchKey: "",
      isSearchBoxUp: false
    };
  },
  methods: {
    searchBoxUp(e) {
      console.log(e);
      this.isSearchBoxUp = !this.isSearchBoxUp;
    },
    clearSerchKey() {
      this.searchKey = "";
    }
  },
  watch: {
    searchKey(oldValue, newValue) {
      if (newValue.length) {
        this.isSearchBoxUp = true;
      }
    }
  }
};
</script>

<style scoped>
/* .enlarge-enter-active,
.enlarge-leave-active {
  transition: all 2s;
}
.enlarge-enter,
.enlarge-leave-to {
  max-height: 0px;
}
.article {
  max-height: 500px;
} */
.button {
  width: 100%;
  height: 40px;
  font-size: 30px;
  text-align: center;
}
.searchbottom {
  position: absolute;
  z-index: 3;
  width: 100%;
  bottom: 0;
  padding: 0 20px 20px;
  background-color: transparent;
  transition: background-color 0.5 ease-out;
}
.searchtop {
  position: absolute;
  z-index: 3;
  width: 100%;
  bottom: 0;
  padding: 0 20px 20px;
  background-color: rgba(255, 255, 255, 0.8);
  transition: background-color 0.5 ease-out;
}
.line {
  border-top: 2px solid black;
  margin: 20px 100px;
}
.box {
  /* justify-content: space-between; */
  height: 38px;
  background-color: white;
  display: flex;
  width: 100%;
  border: 1px solid black;
  font-size: 24px;
}
.input {
  width: 80%;
}
</style>
