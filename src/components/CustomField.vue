<template>
  <div class="hello">
    <div class="tooltip">
      {{ name }}
      <span class="tooltiptext">{{ description }}</span>
    </div>

    <input
      @change="onChange"
      :type="type"
      :placeholder="placeholder"
      v-model="text"

    />
  </div>
</template>

<script>
export default {
  name: "CustomField",
  methods: {
    onChange() {
      if (this.textchanged) {
        this.textchanged();
      }
    },
  },

  computed: {
    text: {
          get() {
                return this.value;
            },
      set(val) {
        this.$emit("input", val);
      },
    },
       validator: {
          get() {
                return this.validate;
            },
      set(e) {
        this.$emit("validator", e);
      },
    },
  },
  props: {
    textchanged: {
      type: Function,
    },
    type: String,
    name: String,
    placeholder: String,
    description: String,
    value: {
      type: String,
    },
  },
};
</script>


<style scoped lang="scss">
input{
  border: 1px solid #ccc;
  padding: 5px;
  border-radius: 5px;
  width: 250px;
}

.tooltip {
  position: relative;
  display: inline-block;
  border-bottom: 1px dotted black;
}

.tooltip .tooltiptext {
  visibility: hidden;
  width: 120px;
  background-color: rgb(255, 255, 255);
  color: rgb(0, 0, 0);
  text-align: center;
  border-radius: 6px;
  padding: 5px 0;

  /* Position the tooltip */
  position: absolute;
  z-index: 1;
}

.tooltip:hover .tooltiptext {
  visibility: visible;
}
</style>
