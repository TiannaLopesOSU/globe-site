<template>
  <div class="container my-5">
    <div class="card">
      <div class="card-header text-center"><h2>Planet Earth!</h2></div>
      <div class="card-body">
        <div><Plane :direction="forwards" /></div>
        <div><Globe /></div>
      </div>
      <div class="card-footer"></div>
    </div>
  </div>
</template>

<script>
import Globe from "../components/GlobeComponent.vue";
import Plane from "../components/PlaneComponent.vue";

export default {
  name: "HomePageView",
  components: {
    Globe,
    Plane,
  },
};
</script>

<style scoped></style>
