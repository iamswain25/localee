<template>
  <v-app>
    <v-alert :value="alertMessage.length" type="error" class="alertMessage">{{alertMessage}}</v-alert>
    <UpperMenu
      :isSearchBoxUp="isSearchBoxUp"
      :searchBoxUp="searchBoxUp"
      :flyToCurrentLocation="flyToCurrentLocation"
      :myArea="myArea"
    />
    <section class="fullScreen">
      <div id="map" class="fullScreen"/>
    </section>
    <SearchBottom
      :isSearchBoxUp="isSearchBoxUp"
      :searchBoxUp="searchBoxUp"
      :searchBoxToggle="searchBoxToggle"
      :setAlertMessage="setAlertMessage"
      :setWriteMode="setWriteMode"
    />
    <WriteTopic v-show="writeMode" :setWriteMode="setWriteMode" :myArea="myArea" :lngLat="lngLat"/>
  </v-app>
</template>

<script>
import mapboxgl from "mapbox-gl";
import "mapbox-gl/dist/mapbox-gl.css";
import MapboxLanguage from "@mapbox/mapbox-gl-language";
mapboxgl.accessToken = process.env.VUE_APP_MAPBOX_ACCESS_TOKEN;
import UpperMenu from "./components/UpperMenu";
import SearchBottom from "./components/SearchBottom";
import WriteTopic from "./components/WriteTopic";
const geocoder = new window.daum.maps.services.Geocoder();
export default {
  name: "App",
  components: {
    UpperMenu,
    SearchBottom,
    WriteTopic,
    lngLat: [0, 0]
  },
  mounted() {
    this.baseMap = new mapboxgl.Map({
      container: "map",
      style: "mapbox://styles/mapbox/basic-v9",
      center: [126.96846939389144, 37.49273748973826],
      zoom: 4
    });
    const language = new MapboxLanguage({
      defaultLanguage: "mul"
    });
    this.baseMap.addControl(language);
    this.getPosition();
    this.baseMap.on("click", function({ lngLat, point }) {
      console.log(lngLat, point);
      const { lng, lat } = lngLat;
      geocoder.coord2RegionCode(lng, lat, (res, status) => {
        const lawArea = res.find(r => r.region_type == "B");
        console.log(lawArea);
        // res.forEach(r => {
        //   let {
        //     region_1depth_name,
        //     region_2depth_name,
        //     region_3depth_name,
        //     region_4depth_name,
        //     region_type
        //   } = r;
        //   console.log({
        //     region_1depth_name,
        //     region_2depth_name,
        //     region_3depth_name,
        //     region_4depth_name,
        //     region_type
        //   });
        // });
      });
      // geocoder.coord2Address(lng, lat, (res, status) => {
      //   const { address } = res[0];
      //   const {
      //     region_1depth_name,
      //     region_2depth_name,
      //     region_3depth_name
      //   } = address;
      //   console.log(region_1depth_name, region_2depth_name, region_3depth_name);
      // });
    });
  },
  data() {
    return {
      isSearchBoxUp: false,
      writeMode: false,
      baseMap: null,
      alertMessage: "",
      geocoder: null,
      myArea: ""
    };
  },
  methods: {
    searchBoxUp(e) {
      console.log(e);
      this.isSearchBoxUp = true;
    },
    searchBoxToggle(e) {
      console.log(e);
      this.isSearchBoxUp = !this.isSearchBoxUp;
    },
    write(e) {
      this.writeMode = true;
      console.time("getCurrentPosition");
      navigator.geolocation.getCurrentPosition(position => {
        console.timeEnd("getCurrentPosition");
        console.log(position);
      });
    },
    setWriteMode() {
      this.writeMode = !this.writeMode;
    },
    async flyToCurrentLocation() {
      console.time("getCurrentPosition");
      navigator.geolocation.getCurrentPosition(position => {
        console.timeEnd("getCurrentPosition");
        console.log(position);
        const { latitude, longitude } = position.coords;
        this.baseMap.flyTo({
          zoom: 12,
          speed: 0.5,
          center: [longitude, latitude]
        });
      });
    },
    setAlertMessage(msg) {
      this.alertMessage = msg;
      window.setTimeout(() => {
        this.alertMessage = "";
      }, 1500);
    },
    async getPosition() {
      navigator.geolocation.getCurrentPosition(
        pos => {
          const { longitude, latitude } = pos.coords;
          geocoder.coord2RegionCode(
            127.10030533139678,
            37.59726687077723,
            (res, status) => {
              const lawArea = res.find(r => r.region_type == "B");
              let {
                region_1depth_name,
                region_2depth_name,
                region_3depth_name,
                region_4depth_name,
                region_type
              } = lawArea;
              const myArea = [
                region_1depth_name,
                region_2depth_name,
                region_3depth_name,
                region_4depth_name
              ]
                .join(" ")
                .trim();
              this.myArea = myArea;
              this.lngLat = [127.10030533139678, 37.59726687077723];
            }
          );
        },
        err => {
          this.setAlertMessage(err.message + " " + "현재위치 설정을 켜시오.");
        },
        {
          maximumAge: Infinity,
          timeout: 3000
        }
      );
    }
  }
};
</script>

<style scoped>
.fullScreen {
  height: 100%;
  width: 100%;
}
.alertMessage {
  position: absolute;
  z-index: 4;
}
</style>
