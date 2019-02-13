<template>
  <transition name="slideup">
    <section :class="isSearchBoxUp ? `searchtop` : `searchbottom` ">
      <div class="line" @click="searchBoxUp"/>
      <div class="box">
        <span>#</span>
        <input type="text" v-model="searchKey">
        <span>X</span>
      </div>
      <article v-show="isSearchBoxUp">
        <div>hash</div>
      </article>
    </section>
  </transition>
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
    }
  }
};
</script>

<style scoped>
.slideup-enter-active, .slideup-leave-active {
  transition: top .5s;
}
.searchbottom {
  position: absolute;
  z-index: 3;
  width: 100%;
  top: 80%;
  padding: 20px 20px;
}
.searchtop {
  position: absolute;
  z-index: 3;
  width: 100%;
  top: 20%;
  padding: 20px 20px;
}
.line {
  border-top: 2px solid black;
  margin: 20px 100px;
}
.box {
  height: 38px;
  background-color: white;
  display: flex;
  width: 100%;
  border: 1px solid black;
  font-size: ;
}
</style>
