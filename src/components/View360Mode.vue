<style>
/* @import "../assets/styles/pannellum.css"; */
#panorama {
  width: 800px;
  height: 600px;
}
#controls {
  position: absolute;
  bottom: 0;
  z-index: 2;
  text-align: center;
  width: 100%;
  padding-bottom: 3px;
}
.ctrl {
  padding: 8px 5px;
  width: 30px;
  text-align: center;
  background: rgba(200, 200, 200, 0.8);
  display: inline-block;
  cursor: pointer;
}
.ctrl:hover {
  background: rgba(200, 200, 200, 1);
}
.custom-hotspot {
  height: 50px;
  width: 50px;
  /*background: #f00;*/
  border: 4px solid yellow;
}
.goto-hotspot {
  height: 50px;
  width: 50px;
  border: 4px solid green;
}
div.custom-tooltip span {
  visibility: hidden;
  position: absolute;
  border-radius: 3px;
  background-color: #fff;
  color: #000;
  text-align: center;
  max-width: 200px;
  padding: 5px 10px;
  margin-left: -220px;
  cursor: default;
}
div.custom-tooltip:hover span {
  visibility: visible;
}
div.custom-tooltip:hover span:after {
  content: "";
  position: absolute;
  width: 0;
  height: 0;
  border-width: 10px;
  border-style: solid;
  border-color: #fff transparent transparent transparent;
  bottom: -20px;
  left: -10px;
  margin: 0 50%;
}
</style>

<template>
  <v-container>
    <div class="text-h3">Bangunan Bersejarah</div>
    <v-pannellum :src="src" style="height: 800px"></v-pannellum>
  </v-container>
</template>

<script>
import Vue from "vue";
import VuePannellum from "vue-pannellum";

Vue.component("VPannellum", VuePannellum);

function customTooltip(hotSpotDiv, args) {
  hotSpotDiv.classList.add("custom-tooltip");
  var span = document.createElement("span");
  span.innerHTML = args;
  hotSpotDiv.appendChild(span);
  span.style.width = span.scrollWidth - 20 + "px";
  span.style.marginLeft =
    -(span.scrollWidth - hotSpotDiv.offsetWidth) / 2 + "px";
  span.style.marginTop = -span.scrollHeight - 12 + "px";
}

// Hot spot creation function
function hotspot(hotSpotDiv, args) {
  customTooltip(hotSpotDiv, args);
}

// Alert Hot spot
function alertMe(hotSpotDiv, args) {
  customTooltip(hotSpotDiv, args);

  hotSpotDiv.addEventListener("click", () => {
    alert("This is pillow alert");
  });
}

export default {
  name: "View360Mode",
  data: () => ({
    src: {
      default: {
        firstScene: "bedroom",
      },
      scenes: {
        bedroom: {
          hfov: 110,
          pitch: -2,
          yaw: 23,
          type: "equirectangular",
          panorama: require("../assets/panorama/tu09tpc-a-large-bed-in-a-room.jpeg"),
          hotSpotDebug: true,
          hotSpots: [
            {
              pitch: -3.05,
              yaw: 20.42,
              type: "scene",
              text: "Living Room",
              sceneId: "livingRoom",
            },
            {
              pitch: 5,
              yaw: -1.8,
              cssClass: "custom-hotspot",
              createTooltipFunc: hotspot,
              createTooltipArgs: "Painting",
            },
            {
              pitch: -6,
              yaw: 46,
              cssClass: "custom-hotspot",
              createTooltipFunc: hotspot,
              createTooltipArgs: "Computer",
            },
            {
              pitch: -22.36,
              yaw: -23.07,
              type: "info",
              createTooltipFunc: alertMe,
              createTooltipArgs: "Pillow",
            },
            {
              pitch: -33.49,
              yaw: 7.63,
              type: "info",
              text: "Books",
            },
          ],
        },
        livingRoom: {
          pitch: -21,
          yaw: -129,
          hfov: 110,
          type: "equirectangular",
          panorama: require("../assets/panorama/791d1c84-a-living-room-in-360.jpeg"),
          autoLoad: true,
          title: "title",
          author: "author",
          compass: true,
          hotSpotDebug: true,
          hotSpots: [
            {
              pitch: -5.4,
              yaw: -118,
              type: "scene",
              text: "Bedroom",
              sceneId: "bedroom",
            },
          ],
        },
      },
    },
  }),
};
</script>
