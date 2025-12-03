<template>
  <div class="my-1">
    <div class="flex flex-col md:flex-row w-full justify-center items-center">

      <div class="block text-center md:order-2 m-1">
        <h2 class="text-center mb-0">STATUS OF EFFLUENT PONDS AND UNLOADING TRACTORS</h2>
      </div>
    </div>
    <div class="flex flex-col md:flex-col w-full justify-center items-center">
      
        <h6 class="text-center mb-0">
        </h6>
      
    </div>
    <horizontal-menu />
  </div>
</template>

<script>
import HorizontalMenu from "./horizontal-menu/HorizontalMenu.vue";
export default {
  components: {
    HorizontalMenu,
  },
  computed: {},
};
</script>

<style scoped>
.header-logo {
  width: 60px;
  height: 60px;
}

.btek-logo {
  width: 40px;
}

@media (max-width: 768px) {
  .header-logo {
    width: 60px;
    height: 60px;
  }
}
</style>
