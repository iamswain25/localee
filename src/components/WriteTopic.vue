<template>
  <section class="writeTopic">
    <div class="topBar">
      <v-icon large @click="setWriteMode">clear</v-icon>
      <div class="area">{{myArea}}</div>
      <div class="smallFont" @click="postContent">POST</div>
    </div>
    <div class="hashTitle">
      <span>양승태</span>
      <span>무죄</span>
    </div>
    <article class="text-container">
      <textarea v-model="content" class="tetxarea" rows="10" maxlength="200"/>
    </article>
  </section>
</template>
<script>
import { firestore, firebase } from "../firebase.js";
const GeoPoint = firebase.firestore.GeoPoint;
export default {
  props: ["setWriteMode", "myArea", "lngLat"],
  data() {
    return {
      content: ""
    };
  },
  methods: {
    postContent() {
      this.setWriteMode();
      const topic = {
        content: this.content,
        area: this.myArea.split(" "),
        coordinates: new GeoPoint(...this.lngLat.reverse()),
        createdAt: new Date(),
        createdBy: "Swain"
      };
      firestore.collection("topics").add(topic);
    }
  },
  watch: {
    content(newVal) {
      console.log(newVal.length);
    }
  }
};
</script>
<style scoped>
.writeTopic {
  height: 100%;
  width: 100%;
  z-index: 5;
  top: 0;
  position: absolute;
  background-color: white;
}
.topBar {
  display: flex;
  justify-content: space-between;
}
.smallFont {
  font-size: 12px;
  padding: 10px;
}
.area {
  font-size: 15px;
  padding: 10px;
  line-height: 20px;
  background-color: rgba(255, 255, 255, 0.8);
}
.hashTitle {
  margin: 20px 20px;
  border-bottom: 1px solid black;
}
.hashTitle span::before {
  content: "#";
}
.hashTitle span {
  padding-left: 5px;
  font-size: 26px;
  font-weight: bold;
}
.tetxarea {
  margin: 20px;
  width: 100%;
  height: 100%;
  margin-top: 0;
  border: none;
  overflow: auto;
  outline: none;
  -webkit-box-shadow: none;
  -moz-box-shadow: none;
  box-shadow: none;
  resize: none;
}
.text-container {
  display: flex;
  justify-content: center;
}
</style>
