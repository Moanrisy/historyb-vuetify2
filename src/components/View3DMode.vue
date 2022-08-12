<style>
body {
  margin: 0;
}
canvas {
  display: block;
}
</style>
<template>
  <div ref="data3d"></div>
</template>

<script>
// import * threejs as from "../assets/js/three.js";
import * as THREE from "three";
import "../assets/js/GLTFLoader";
import { GLTFLoader } from "../assets/js/GLTFLoader";

export default {
  mounted: function () {
    this.$nextTick(function () {
      this.$refs.data3d.appendChild(renderer.domElement);
    });
  },
  data: () => ({
    data3d: null,
  }),
};

var scene = new THREE.Scene();
var camera = new THREE.PerspectiveCamera(
  75,
  window.innerWidth / window.innerHeight,
  0.01,
  1000
);

var renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight);
// document.body.appendChild(renderer.domElement);

var loader = new GLTFLoader();

var obj;
loader.load("3d/real_car_22/scene.gltf", function (gltf) {
  obj = gltf.scene;
  scene.add(gltf.scene);
});

scene.background = new THREE.Color(0xffffff);
var light = new THREE.HemisphereLight(0xffffff, 0x000000, 10);
scene.add(light);
camera.position.set(-2, 2, 6);
function animate() {
  requestAnimationFrame(animate);
  if (obj) obj.rotation.y += 0.01;
  renderer.render(scene, camera);
}

animate();
</script>
