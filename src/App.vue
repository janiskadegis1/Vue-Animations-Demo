<template>
  <div id="app" :style="cssVars">
    <CustomHeader title="Animations Demo" @css-prop-change="setCssProperty" :cssProps="cssProps" />
    <CustomBody msg="Welcome to Your Vue.js App" />
  </div>
</template>

<script>
import CustomBody from "./components/CustomBody.vue";
import CustomHeader from "./components/CustomHeader.vue";
export default {
  name: "app",
  components: {
    CustomBody,
    CustomHeader
  },
  data: function() {
    return {
      // default values of editable css properties
      cssProps: {
        duration: "0.4s",
        "timing-func": "ease-in-out",
        "color-white": "#fff9fb",
        "color-gray": "#d3d4d9",
        "color-blue": "#4b88a2",
        "color-red": "#bb0a21",
        "color-black": "#252627"
      }
    };
  },
  computed: {
    cssVars() {
      return {
        "--duration": this.cssProps.duration,
        "--timing-func": this.cssProps["timing-func"],
        "--color-white": this.cssProps["color-white"],
        "--color-gray": this.cssProps["color-gray"],
        "--color-blue": this.cssProps["color-blue"],
        "--color-red": this.cssProps["color-red"],
        "--color-black": this.cssProps["color-black"]
      };
    }
  },
  methods: {
    setCssProperty(event) {
      this.cssProps[event.key] = event.value;
    }
  }
};
</script>

<style>

#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background: var(--color-white);
  min-height: 100vh;
}

</style>
