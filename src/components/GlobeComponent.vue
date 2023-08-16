<template>
  <div class="container">
    <div><div id="globeCanvasContainer"></div></div>
  </div>
</template>

<script>
import * as THREE from "three";
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader.js";

export default {
  name: "GlobeComponent",
  data() {
    return {
      camera: null,
      renderer: null,
      scene: null,
    };
  },
  mounted() {
    const globeUrl = new URL("../assets/globe.glb", import.meta.url);
    const canvasContainer = document.getElementById("globeCanvasContainer");

    const renderer = new THREE.WebGLRenderer();
    this.renderer = renderer;
    renderer.shadowMap.enabled = true;
    renderer.setSize(window.innerWidth * 0.5, window.innerHeight * 0.5);
    canvasContainer.appendChild(renderer.domElement);

    const scene = new THREE.Scene();
    this.scene = scene;
    scene.background = new THREE.Color(0xffffff);
    const camera = new THREE.PerspectiveCamera(
      3,
      window.innerWidth / window.innerHeight,
      0.1,
      1000
    );
    this.camera = camera;
    camera.position.set(10, 0, 50); // Move camera closer along the Z-axis
    camera.lookAt(0, 0, 0);

    const ambientLight = new THREE.AmbientLight(0xfffffff);
    scene.add(ambientLight);

    const assetLoader = new GLTFLoader();
    let model;
    assetLoader.load(
      globeUrl.href,
      (gltf) => {
        model = gltf.scene;
        scene.add(model);
        model.position.set(0, 0, 0);
      },
      undefined,
      (error) => {
        console.error(error);
      }
    );

    function animate() {
      // Rotate the camera around the model
      camera.position.x = Math.sin(Date.now() * -0.001) * 50; // Adjust the radius
      camera.position.z = Math.cos(Date.now() * -0.001) * 50; // Adjust the radius
      camera.lookAt(0, 0, 0);

      // Call the animate function to keep rendering
      renderer.render(scene, camera);
      requestAnimationFrame(animate);
    }

    animate();

    window.addEventListener("resize", () => {
      camera.aspect = window.innerWidth / window.innerHeight;
      camera.updateProjectionMatrix();
      renderer.setSize(window.innerWidth * 0.5, window.innerHeight * 0.5);
    });
  },
};
</script>

<style scoped></style>
