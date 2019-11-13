<template>
  <div class="main-body">
    <div class="control-row">
      <div class="input-group">
        <input type="text" placeholder="Enter item title" v-model="inputValue" />
      </div>
      <button @click="addValue">Add Item</button>
    </div>
    <draggable v-model="itemArray">
      <transition-group>
        <div
          v-for="element in itemArray"
          :key="element.id"
          class="draggable-table-item"
        >{{element.name}}</div>
      </transition-group>
    </draggable>
  </div>
</template>

<script>
import draggable from "vuedraggable";
export default {
  name: "HeightTest",
  props: {
    msg: String,
    itemArray: Array
  },
  data: function() {
    return {
      inputValue: ""
    };
  },
  methods: {
    addValue() {
      if (this.inputValue) {
        const randomID = Math.floor(Math.random() * 99999);
        this.itemArray.push({
          id: randomID,
          name: this.inputValue
        });
        this.inputValue = "";
      }
    }
  },
  components: {
    draggable
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.main-body {
  width: calc(100% - 60px);
  padding: 30px;
}

.control-row {
  display: flex;
  width: 100%;
  padding-bottom: 20px;
}

.input-group input {
  width: 10rem;
  height: 2.5em;
  padding: 0 0.5rem;
  border-radius: 4px;
  border: solid 1px var(--color-blue);
  font-weight: bold;
  box-shadow: 0 0 8px 1px rgba(0, 0, 0, 0);
  transition: box-shadow var(--duration) var(--timing-func);
  margin-right: 2em;
}

.input-group input:focus {
  outline: none;
  box-shadow: 0 0 8px 1px var(--color-blue);
}

.control-row button {
  border: none;
  background: var(--color-blue);
  color: var(--color-white);
  border-radius: 4px;
}

.control-row button:focus {
  outline: none;
  box-shadow: 0 0 8px 1px var(--color-blue);
}

.draggable-table-item {
  width: 10em;
  padding: 0.5em;
  height: 1em;
  line-height: 1em;
  margin-top: -1px;
  border: solid 1px var(--color-black);
}
</style>
