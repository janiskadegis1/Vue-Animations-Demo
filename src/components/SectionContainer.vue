<template>
  <div class="section-container" :class="{active: isActive}">
    <div class="header" @click="toggleActive">
      Dropdown template
      <div class="collapse-btn"></div>
    </div>
    <transition name="testing">
      <div class="section-body" v-if="isActive">
        <HeightTest v-if="type === 'heightTest'" :itemArray="itemArray" />
      </div>
    </transition>
  </div>
</template>

<script>
import HeightTest from "./sections/HeightTest.vue";
export default {
  name: "SectionContainer",
  props: {
    type: String
  },
  data: function() {
    return {
      isActive: true,
      itemArray: [
        {
          id: "1",
          name: "Item #1"
        },
        {
          id: "2",
          name: "Item #2"
        },
        {
          id: "3",
          name: "Another Item"
        },
        {
          id: "4",
          name: "xxxxxx"
        }
      ]
    };
  },
  methods: {
    toggleActive() {
      this.isActive = !this.isActive;
    }
  },
  components: {
    HeightTest
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.section-container {
  width: 100%;
  height: fit-content;
  border-radius: 10px;
  box-shadow: 2px 2px 10px 3px rgba(0, 0, 0, 0.1);
  user-select: none;
}

.header {
  color: var(--color-white);
  background: var(--color-blue);
  border-radius: inherit;
  padding: 0.7em 2em;
  position: relative;
  cursor: pointer;
}

.collapse-btn {
  width: 2.8em;
  height: 100%;
  position: absolute;
  right: 0;
  top: 0;
}

.collapse-btn::after {
  position: absolute;
  content: "";
  border-right: 3px solid var(--color-white);
  border-bottom: 3px solid var(--color-white);
  width: 0.8em;
  height: 0.8em;
  transform: rotate(45deg);
  top: 25%;
  left: 30%;
  transition: all var(--duration) var(--timing-func);
}

.active .collapse-btn::after {
  transform: rotate(-135deg);
  top: 40%;
  left: 30%;
}

.section-body {
  width: 100%;
  height: fit-content;
  background: var(--color-white);
  border-radius: 0 0 10px 10px;
}

.testing-enter-active {
  animation: enter var(--duration) ease-in;
}

.testing-leave-active {
  animation: leave var(--duration) ease-out;
}

@keyframes enter {
  0% {
    overflow: hidden;
    max-height: 0px;
  }
  99% {
    max-height: 90vh;
    overflow: hidden;
  }
  100% {
    overflow: visible;
    max-height: unset;
  }
}
@keyframes leave {
  0% {
    overflow: visible;
    max-height: unset;
  }
  1% {
    overflow: hidden;
    max-height: 90vh;
  }
  100% {
    overflow: hidden;
    max-height: 0px;
  }
}
</style>
