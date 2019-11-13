<template>
  <div class="header-container is-visible" :class="{active: isActive}">
    {{ title}}
    <div class="config-dropdown-btn" @click.stop="toggleActive">
      <div class="dropdown-icon"></div>
    </div>
    <div class="header-dropdown" v-click-outside="closeDropdown" @click="toggleColorPicker()">
      <div class="dropdown-section">
        <label>Animation duration</label>
        <input
          type="text"
          :value="cssProps.duration"
          v-on:change="changeCssProp($event, 'duration')"
        />
      </div>
      <div class="dropdown-section">
        <label>Timing function</label>
        <input
          type="text"
          :value="cssProps['timing-func']"
          v-on:change="changeCssProp($event, 'timing-func')"
        />
      </div>

      <div class="dropdown-section colors" :class="{open: colorsCollapsed}">
        <label>Colors</label>
        <div class="collapse-btn" @click="toggleColorsCollapse"></div>
        <transition name="collapsible">
          <div class="collapsible-input-group" v-if="colorsCollapsed">
            <div class="dropdown-section" v-for="color in colors" :key="color">
              <label>{{ color | capitalize }}</label>
              <input
                type="text"
                :value="cssProps['color-' + color]"
                v-on:change="changeCssProp($event, 'color-' + color)"
              />
              <div
                class="color-preview"
                :style="{background: 'var(--color-' + color + ')'}"
                @click.stop="toggleColorPicker($event, color)"
              >
                <transition name="color-picker">
                  <color-picker
                    :value="cssProps['color-' + color]"
                    class="color-picker"
                    v-if="activeColorPicker === color"
                    @input="updateColor($event, color)"
                  />
                </transition>
              </div>
            </div>
          </div>
        </transition>
      </div>
    </div>
  </div>
</template>

<script>
import { Chrome } from "vue-color";

export default {
  name: "CustomHeader",
  data: function() {
    return {
      isActive: false,
      colorsCollapsed: false,
      activeColorPicker: null
    };
  },
  computed: {
    colors() {
      return Object.keys(this.cssProps)
        .filter(item => item.startsWith("color-"))
        .map(item => item.split("-")[1]);
    }
  },
  filters: {
    capitalize: function(value) {
      if (!value) return "";
      value = value.toString();
      return value.charAt(0).toUpperCase() + value.slice(1);
    }
  },
  methods: {
    handleSCroll() {
      let header = document.querySelector(".header-container");
      if (window.scrollY < 60) {
        header.classList.add("is-visible");
      } else if (window.scrollY > 60) {
        header.classList.remove("is-visible");
      }
    },
    toggleActive() {
      this.isActive = !this.isActive;
    },
    closeDropdown() {
      this.isActive = false;
      this.colorsCollapsed = true;
      this.activeColorPicker = null;
    },
    toggleColorsCollapse() {
      this.colorsCollapsed = !this.colorsCollapsed;
    },
    toggleColorPicker(event, color) {
      console.log("toggleColorPicker", event, color);
      if (color && this.activeColorPicker !== color) {
        this.activeColorPicker = color;
      } else if (
        !event ||
        !event.path.find(item => item.className === "color-picker vc-chrome")
      ) {
        this.activeColorPicker = null;
      }
    },
    changeCssProp(e, prop) {
      this.$emit("css-prop-change", { key: prop, value: e.target.value });
    },
    updateColor(value, colorName) {
      this.$emit("css-prop-change", {
        key: "color-" + colorName,
        value: value.hex8
      });
    }
  },
  created() {
    window.addEventListener("scroll", this.handleSCroll);
  },
  destroyed() {
    window.removeEventListener("scroll", this.handleSCroll);
  },
  props: {
    title: String,
    cssProps: Object
  },
  components: {
    "color-picker": Chrome
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
/****************************************/
/*           Main Header styles         */
/****************************************/
.header-container {
  position: fixed;
  top: 0;
  left: 0;
  width: calc(100vw - 3em);
  height: 1.5em;
  background: var(--color-gray);
  font-size: 2em;
  line-height: 1.5em;
  padding-left: 3em;
  font-weight: bold;
  transition: all var(--duration) var(--timing-func);
  z-index: 10;
  user-select: none;
}
.header-container.is-visible {
  height: 2.5em;
  line-height: 2.5em;
  font-size: 2em;
}

/****************************************/
/*             Dropdown Button          */
/****************************************/
.config-dropdown-btn {
  position: absolute;
  width: 2.5em;
  height: 100%;
  top: 0;
  right: 0;
  cursor: pointer;
}

.dropdown-icon {
  background: var(--color-white);
  position: absolute;
  width: 1em;
  height: 5px;
  top: 0.7em;
  right: 20px;
  transition: all var(--duration) var(--timing-func);
}

.is-visible .dropdown-icon {
  top: 1.2em;
}

.dropdown-icon::after,
.dropdown-icon::before {
  content: "";
  background: var(--color-white);
  position: absolute;
  width: 1em;
  height: 5px;
  right: 0;
  transition: all var(--duration) var(--timing-func);
}

.dropdown-icon::before {
  top: -10px;
}

.dropdown-icon::after {
  bottom: -10px;
}

.active .dropdown-icon {
  background: rgba(255, 255, 255, 0);
}

.active .dropdown-icon::after {
  transform: rotate(-45deg);
  bottom: 0;
  right: -5px;
  width: 0.8em;
}

.active .dropdown-icon::before {
  transform: rotate(45deg);
  top: 0;
  right: 10px;
  width: 0.8em;
}

/****************************************/
/*           Header Dropdown            */
/****************************************/
.header-dropdown {
  background: var(--color-white);
  width: 8em;
  position: absolute;
  top: 100%;
  right: 0;
  box-shadow: -4px 5px 9px 0px rgba(0, 0, 0, 0.15);
  border-bottom-left-radius: 5px;
  clip-path: circle(0% at 85% -30px);
  transition: all var(--duration) var(--timing-func);
  padding: 1em 1em calc(1em - 20px) 1em;
}

.active .header-dropdown {
  clip-path: circle(160% at 85% -30px);
}

.dropdown-section {
  display: flex;
  flex-direction: column;
  margin-bottom: 20px;
}

.dropdown-section label {
  font-size: 0.4em;
  line-height: 1.2em;
  margin-bottom: 5px;
  font-weight: bold;
}

.dropdown-section input {
  width: calc(100% - 28px);
  height: 2.5em;
  padding: 0 14px;
  border-radius: 4px;
  border: solid 1px var(--color-blue);
  font-weight: bold;
  box-shadow: 0 0 8px 1px rgba(0, 0, 0, 0);
  transition: box-shadow var(--duration) var(--timing-func);
}

.dropdown-section input:focus {
  outline: none;
  box-shadow: 0 0 8px 1px var(--color-blue);
}

/****************************************/
/*       Collapsible Input Group        */
/****************************************/
@keyframes leave {
  0% {
    overflow: visible;
    max-height: unset;
  }
  1% {
    overflow: hidden;
    max-height: 380px;
  }
  100% {
    overflow: hidden;
    max-height: 0px;
    padding: 0 10px;
  }
}

@keyframes enter {
  0% {
    overflow: hidden;
    max-height: 0px;
    padding: 0 10px;
  }
  99% {
    max-height: 380px;
  }
  100% {
    overflow: visible;
    max-height: unset;
  }
}

.collapsible-input-group {
  width: calc(100% - 20px);
  border-bottom: 1px dotted var(--color-black);
  padding: 10px 10px 0 10px;
  position: relative;
}

.dropdown-section.colors {
  position: relative;
  border-top: 1px dotted var(--color-black);
  margin-top: 1em;
}

.dropdown-section.colors > label {
  margin-top: -1.4em;
}

.collapse-btn {
  position: absolute;
  width: 25px;
  height: 25px;
  background: var(--color-white);
  top: -10px;
  right: 0;
  cursor: pointer;
  z-index: 2;
}

.collapse-btn:after {
  position: absolute;
  content: "";
  border-right: 2px solid var(--color-black);
  border-bottom: 2px solid var(--color-black);
  width: 8px;
  height: 8px;
  transform: rotate(45deg);
  top: 3px;
  left: 8px;
  transition: all var(--duration) var(--timing-func);
}

.open .collapse-btn::after {
  transform: rotate(-135deg);
  top: 6px;
  left: 8px;
}

.collapsible-enter-active {
  animation: enter var(--duration) ease-in;
}

.collapsible-leave-active {
  animation: leave var(--duration) ease-out;
}

.colors input {
  padding-right: 28px;
  width: calc(100% - 42px);
}
.colors .dropdown-section {
  position: relative;
}

/****************************************/
/*             Color Picker             */
/****************************************/

.color-preview {
  position: absolute;
  content: "";
  width: 0.5em;
  height: 0.5em;
  right: 4px;
  top: 26px;
  border-radius: 50%;
  border: 1px solid black;
}

.color-picker {
  position: absolute;
  right: 0px;
  top: 24px;
  z-index: 2;
}
</style>
