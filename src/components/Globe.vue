<template>
  <div class="container">
    Globe Component
    <div id="threeCanvasContainer"></div>
  </div>
</template>

<script>
import * as THREE from "three";
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader.js";
export default {
  name: "GlobeComponent",
  data() {
    return {};
  },
  mounted() {
    const narratorUrl = new URL("../assets/globe.glb", import.meta.url);
    const canvasContainer = document.getElementById("threeCanvasContainer");

    const renderer = new THREE.WebGLRenderer();
    renderer.shadowMap.enabled = true;
    renderer.setSize(window.innerWidth * 0.5, window.innerHeight * 0.5);
    canvasContainer.appendChild(renderer.domElement);

    const scene = new THREE.Scene();
    scene.background = new THREE.Color(0xffffff);
    const camera = new THREE.PerspectiveCamera(
      45,
      window.innerWidth / window.innerHeight,
      0.1,
      1000
    );
    camera.position.set(5, 6, 9);
    camera.lookAt(0, 0, 0);

    const ambientLight = new THREE.AmbientLight(0xfffffff);
    scene.add(ambientLight);

    const assetLoader = new GLTFLoader();
    let model;
    assetLoader.load(
      narratorUrl.href,
      function (gltf) {
        model = gltf.scene;
        scene.add(model);
        model.position.set(3, 4, 10);
        const box = new THREE.Box3().setFromObject(model);
        const center = box.getCenter(new THREE.Vector3());
        camera.lookAt(center);
      },
      undefined,
      function (error) {
        console.error(error);
      }
    );

    function animate() {
      requestAnimationFrame(animate);

      renderer.render(scene, camera);
    }

    renderer.setAnimationLoop(animate);

    window.addEventListener("resize", function () {});
  },
};
</script>

<style scoped></style>
