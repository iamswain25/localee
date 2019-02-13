<template>
  <v-app>
    <UpperMenu/>
    <v-content>
      <BaseMap/>
    </v-content>
    <SearchBottom/>
  </v-app>
</template>

<script>
import BaseMap from "./components/BaseMap";
import UpperMenu from "./components/UpperMenu";
import SearchBottom from "./components/SearchBottom";
export default {
  name: "App",
  components: {
    BaseMap,
    UpperMenu,
    SearchBottom
  },
  data() {
    return {
      //
    };
  }
};
</script>
