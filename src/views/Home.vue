<template>
  <div class="flex flex-col h-screen max-h-screen">
    <div
      class="
        z-20
        flex
        justify-center
        relative
        bg-hero-pattern bg-cover
        px-4
        pt-8
        pb-32
      "
    >
      <div class="w-full max-w-screen-sm">
        <h1 class="text-white text-center text-3xl pb-4">IP Address Tracker</h1>
        <div class="flex">
          <input
            v-model="queryIP"
            type="text"
            placeholder="Search for any IP or just leave empty to get your location"
            class="
              flex-1
              py-3
              px-2
              rounded-tl-md rounded-bl-md
              focus:outline-none
            "
          />
          <i
            @click="getIpInfo"
            class="
              fa fa-chevron-right
              cursor-pointer
              bg-black
              text-white
              px-4
              rounded-tr-md rounded-br-md
              flex
              items-center
            "
          ></i>
        </div>
      </div>

      <IPinfo v-if="IpInfo" :IpInfo="IpInfo" />
    </div>
    <div id="mapid" class="h-full z-10"></div>
  </div>
</template>

<script>
import IPinfo from "../components/IPinfo.vue";
import leaflet from "leaflet";
import { onMounted, ref } from "@vue/runtime-core";
import axios from "axios";
export default {
  name: "Home",
  components: { IPinfo },
  setup() {
    let mymap;
    const queryIP = ref("");
    const IpInfo = ref(null);
    onMounted(() => {
      mymap = leaflet.map("mapid").setView([51.505, -0.09], 9);

      leaflet
        .tileLayer(
          "https://api.mapbox.com/styles/v1/{id}/tiles/{z}/{x}/{y}?access_token=pk.eyJ1Ijoibm9vZ2FpIiwiYSI6ImNrdTVkcmNlYTE3enEydm82cDY0c3pyd3cifQ.dh2Vu3NJyfyJlMssk0Gopw",
          {
            attribution:
              'Map data &copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors, Imagery © <a href="https://www.mapbox.com/">Mapbox</a>',
            maxZoom: 18,
            id: "mapbox/streets-v11",
            tileSize: 512,
            zoomOffset: -1,
            accessToken:
              "pk.eyJ1Ijoibm9vZ2FpIiwiYSI6ImNrdTVkcmNlYTE3enEydm82cDY0c3pyd3cifQ.dh2Vu3NJyfyJlMssk0Gopw",
          }
        )
        .addTo(mymap);
    });

    const getIpInfo = async () => {
      try {
        const data = await axios.get(
          `https://geo.ipify.org/api/v1?apiKey=at_E2UYLaaMGUUvYZ8fQ8Inyaa56g1X0&ipAddress=${queryIP.value}`
        );
        const result = data.data;
        console.log(result);
        IpInfo.value = {
          address: result.ip,
          state: result.location.region,
          city: result.location.city,
          timezone: result.location.timezone,
          isp: result.isp,
          lat: result.location.lat,
          lng: result.location.lng,
        };
        leaflet.marker([IpInfo.value.lat, IpInfo.value.lng]).addTo(mymap);
        mymap.setView([IpInfo.value.lat, IpInfo.value.lng], 13);
      } catch (err) {
        alert(err.message);
      }
    };

    return { queryIP, IpInfo, getIpInfo };
  },
};
</script>
