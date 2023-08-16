<template>
  <div>
    <div><div id="planeCanvasContainer"></div></div>
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
  props: {
    direction: String,
  },
  mounted() {
    const planeUrl = new URL("../assets/plane.glb", import.meta.url);
    const canvasContainer = document.getElementById("planeCanvasContainer");

    var renderer = new THREE.WebGLRenderer({ alpha: true });
    renderer.setClearColor(0x000000, 0);

    this.renderer = renderer;

    renderer.shadowMap.enabled = true;
    renderer.setSize(window.innerWidth * 0.3, window.innerHeight * 0.3);
    canvasContainer.appendChild(renderer.domElement);

    const scene = new THREE.Scene();
    this.scene = scene;
    scene.background = new THREE.Color(0xffffff);
    const camera = new THREE.PerspectiveCamera(
      45,
      window.innerWidth / window.innerHeight,
      0.1,
      1000
    );
    this.camera = camera;

    camera.position.set(-26, 0, 0);
    camera.lookAt(0, 0, 0);

    const ambientLight = new THREE.AmbientLight(0xfffffff);
    scene.add(ambientLight);

    const assetLoader = new GLTFLoader();
    let model;

    assetLoader.load(
      planeUrl.href,
      (gltf) => {
        model = gltf.scene;
        scene.add(model);
        model.position.set(-20, 0, 0);
        model.scale.set(0.1, 0.1, 0.1);
      },
      undefined,
      (error) => {
        console.error(error);
      }
    );

    function animate() {
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
  methods: {},
};
</script>

<style scoped></style>
