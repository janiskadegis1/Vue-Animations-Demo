<template>
  <div class="main-body">
    <SectionContainer type="heightTest" />
  </div>
</template>

<script>
import SectionContainer from "./SectionContainer.vue";
export default {
  name: "CustomBody",
  props: {
    msg: String
  },
  components: { SectionContainer }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.main-body {
  padding: 120px 80px;
  width: calc(100% - 160px);
  height: 1000px;
}
</style>
