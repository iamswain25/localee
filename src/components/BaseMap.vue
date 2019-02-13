<template>
  <section class="fullScreen">
    <div id="map" class="fullScreen"/>
  </section>
</template>

<script>
import mapboxgl from "mapbox-gl";
import "mapbox-gl/dist/mapbox-gl.css";
import MapboxLanguage from "@mapbox/mapbox-gl-language";
mapboxgl.accessToken = process.env.VUE_APP_MAPBOX_ACCESS_TOKEN;
const geocoder = new window.daum.maps.services.Geocoder();
export default {
  mounted() {
    var map = new mapboxgl.Map({
      container: "map",
      style: "mapbox://styles/mapbox/basic-v9",
      center: [126.96846939389144, 37.49273748973826],
      zoom: 4
    });
    const language = new MapboxLanguage({
      defaultLanguage: "mul"
    });
    map.addControl(language);
    // map.on("load", () => {
    //   map.flyTo({ zoom: 12, speed: 0.2 });
    // });

    map.on("click", function({ lngLat, point }) {
      console.log(lngLat, point);
      const { lng, lat } = lngLat;
      geocoder.coord2RegionCode(lng, lat, (res, status) => {
        res.forEach(r => {
          let {
            region_1depth_name,
            region_2depth_name,
            region_3depth_name,
            region_4depth_name,
            region_type
          } = r;
          console.log({
            region_1depth_name,
            region_2depth_name,
            region_3depth_name,
            region_4depth_name,
            region_type
          });
        });
      });
      geocoder.coord2Address(lng, lat, (res, status) => {
        console.log(res[0]);
      });
    });
  },
  data() {
    return {
      writeMode: false
    };
  },
  methods: {
    write(e) {
      this.writeMode = true;
      console.time("getCurrentPosition");
      navigator.geolocation.getCurrentPosition(position => {
        console.timeEnd("getCurrentPosition");
        console.log(position);
      });
    }
  }
};
</script>

<style scoped>
.fullScreen {
  height: 100%;
  width: 100%;
}
.floatingIcon {
  position: absolute;
  bottom: 15px;
  right: 5px;
}
</style>
