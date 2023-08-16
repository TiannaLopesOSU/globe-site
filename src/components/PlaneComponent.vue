<template>
  <div class="container">
    <div><div id="planeCanvasContainerForwards"></div></div>
    <div><div id="planeCanvasContainerBackwards"></div></div>
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
      rendererForwards: null,
      rendererBackwards: null,
      scene: null,
    };
  },
  props: {
    direction: String,
  },
  mounted() {
    const planeUrl = new URL("../assets/plane.glb", import.meta.url);
    const canvasContainerForwards = document.getElementById(
      "planeCanvasContainerForwards"
    );
    const canvasContainerBackwards = document.getElementById(
      "planeCanvasContainerBackwards"
    );

    const rendererForwards = new THREE.WebGLRenderer();
    const rendererBackwards = new THREE.WebGLRenderer();

    this.rendererForwards = rendererForwards;
    this.rendererBackwards = rendererBackwards;

    if (this.direction == "forward") {
      rendererForwards.shadowMap.enabled = true;
      rendererForwards.setSize(
        window.innerWidth * 0.5,
        window.innerHeight * 0.2
      );
      canvasContainerForwards.appendChild(rendererForwards.domElement);
    } else {
      rendererBackwards.shadowMap.enabled = true;
      rendererBackwards.setSize(
        window.innerWidth * 0.5,
        window.innerHeight * 0.2
      );
      canvasContainerBackwards.appendChild(rendererBackwards.domElement);
    }

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
      planeUrl.href,
      (gltf) => {
        model = gltf.scene;
        scene.add(model);
        model.position.set(-20, 0, 0);
        model.scale.set(0.2, 0.2, 0.2);
      },
      undefined,
      (error) => {
        console.error(error);
      }
    );

    function animate() {
      if (this.direction === "forwards") {
        // Rotate the camera around the model
        camera.position.x = Math.sin(Date.now() * -0.001) * 50; // Adjust the radius
        camera.position.z = Math.cos(Date.now() * -0.001) * 50; // Adjust the radius
        camera.lookAt(0, 0, 0);
      } else {
        // Rotate the camera around the model
        camera.position.x = Math.sin(Date.now() * 0.001) * 50; // Adjust the radius
        camera.position.z = Math.cos(Date.now() * 0.001) * 50; // Adjust the radius
        camera.lookAt(0, 0, 0);
      }

      // Call the animate function to keep rendering
      if (this.direction === "forward") {
        rendererForwards.render(scene, camera);
      } else {
        rendererBackwards.render(scene, camera);
      }
      requestAnimationFrame(animate);
    }

    animate();

    window.addEventListener("resize", () => {
      camera.aspect = window.innerWidth / window.innerHeight;
      camera.updateProjectionMatrix();
      if (this.direction === "forward") {
        rendererForwards.setSize(
          window.innerWidth * 0.5,
          window.innerHeight * 0.5
        );
      } else {
        rendererBackwards.setSize(
          window.innerWidth * 0.5,
          window.innerHeight * 0.5
        );
      }
    });
  },
};
</script>

<style scoped></style>
