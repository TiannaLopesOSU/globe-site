<template>
  <div class="container">
    Globe Component
    <div id="globe-container"></div>
  </div>
</template>

<script>
import * as THREE from "three";
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader.js";
export default {
  name: "GlobeComponent",
  data() {
    return {
      globeUrl: new URL("../assets/globe.glb", import.meta.url),
    };
  },
  mounted() {
    const renderer = new THREE.WebGLRenderer();
    renderer.shadowMap.enabled = true;
    renderer.setSize(window.innerWidth * 0.5, window.innerHeight * 0.5);
    this.$el.querySelector("#globe-container").appendChild(renderer.domElement);

    const scene = new THREE.Scene();

    const camera = new THREE.PerspectiveCamera(
      45,
      window.innerWidth / window.innerHeight,
      0.1,
      1000
    );
    camera.position.set(5, 6, 9);
    camera.lookAt(0, 0, 0);
    const assetLoader = new GLTFLoader();
    const light = new THREE.SpotLight(0xffffff);
    light.position.set(100, 100, -100);
    light.castShadow = true;
    light.angle = 0.2;
    scene.add(light);

    assetLoader.load(
      this.globeUrl.href,
      function (gltf) {
        const model = gltf.scene;
        scene.add(model);
        model.position.set(-3, 4, 9);
        const box = new THREE.Box3().setFromObject(model);
        const center = box.getCenter(new THREE.Vector3());
        camera.lookAt(center);

        // const mesh = gltf.scene.children[0];
        console.log(gltf.scene);
        // const shapeKeys = mesh.morphTargetInfluences; // Access the shape keys/morph targets
        // console.log(shapeKeys);
      },
      undefined,
      function (error) {
        console.error(error);
      }
    );
    renderer.render(scene, camera);
  },
};
</script>

<style scoped></style>
