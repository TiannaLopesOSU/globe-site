<template>
  <div class="container my-5">
    <div class="card">
      <div class="card-header text-center"><h2>Planet Earth!</h2></div>
      <div class="card-body">
        <div ref="moveableDiv" class="moveableDiv">
          <div><Plane :direction="forwards" /></div>
        </div>
        <div><Globe /></div>
      </div>
      <div class="card-footer d-flex justify-content-center">
        <button @click="moveDiv" class="btn btn-success">
          Send plane away!!
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import Globe from "../components/GlobeComponent.vue";
import Plane from "../components/PlaneComponent.vue";
import { gsap } from "gsap";

export default {
  name: "HomePageView",
  components: {
    Globe,
    Plane,
  },
  methods: {
    moveDiv() {
      const moveableDiv = this.$refs.moveableDiv;
      const screenWidth = window.innerWidth;
      const screenHeight = window.innerHeight;

      let randomX = Math.random() * (screenWidth - moveableDiv.clientWidth);
      let randomY = Math.random() * (screenHeight - moveableDiv.clientHeight);

      gsap.to(moveableDiv, {
        duration: 1,
        x: randomX,
        y: randomY,
        repeat: -1,
        onRepeat: () => {
          const randomX =
            Math.random() * (screenWidth - moveableDiv.clientWidth);
          const randomY =
            Math.random() * (screenHeight - moveableDiv.clientHeight);

          gsap.to(moveableDiv, {
            duration: 1,
            x: randomX,
            y: randomY,
          });

          console.log("Movement animation completed and repeated");
        },
      });
    },
  },
  mounted() {
    this.moveDiv();
  },
};
</script>

<style scoped>
.moveable-div {
  margin: 20px;
}
</style>
