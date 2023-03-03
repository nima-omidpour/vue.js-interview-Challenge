<template>
  <div
    class="multiselect"
    @click="showOptions"
    @blur="focused = false"
    tabindex="1"
    ref="parent"
    :style="{ width: width }"
  >
    <span v-if="!existsValue"> please select specializations...</span>

    <div
      class="multiselect__selected"
      v-for="(option, i) in formattedOptions"
      :key="i"
      v-show="option.checked"
    >
      {{ option.option }}
      <span
        class="multiselect__remove"
        @click="
          preventClose($event);
          selectOption(i);
        "
        >&times;</span
      >
    </div>
    <div
      class="multiselect__options"
      v-show="focused"
      :style="{ top: optionsTop }"
      @click="preventClose"
    >
      <div
        class="multiselect__option"
        :class="{ 'multiselect__option--checked': option.checked }"
        v-for="(option, i) in formattedOptions"
        :key="i"
        @click="selectOption(i)"
      >
        <div class="multiselect__checkbox"></div>
        <div class="multiselect__text">
          {{ option.option }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    width: {
      type: String,
      default: "100%",
    },
    options: {
      type: Array,
      default: () => [],
    },
    value: {
      default: () => [],
    },
    placeholder: {
      type: String,
      default: "click to select",
    },
    displayProperty: {
      type: String,
      default: "name",
    },
    valueProperty: {
      type: String,
      default: "name",
    },

    required: {
      default: false,
    },
  },
  data() {
    return {
      focused: false,
      optionsTop: "58px",
    };
  },
  methods: {
    fixTop() {
      this.optionsTop = this.$refs.parent.clientHeight + 2 + "px";
    },
    showOptions() {
      this.focused = !this.focused;
    },
    preventClose(e) {
      e.stopPropagation();
    },
    selectOption(i) {
      let clickedValue = this.options[i];
      let newValue = [...this.value];

      let existIndex = this.value.findIndex((v) => v === clickedValue);

      if (existIndex === -1) {
        newValue.push(clickedValue);
      } else {
        newValue.splice(existIndex, 1);
      }

      this.$emit("input", newValue);
      setTimeout(this.fixTop, 100);
    },
  },
  computed: {
    formattedOptions() {
      let fo = this.options.map((option) => {
        let checked = this.value.some((v) => v === option);
        return { option, checked };
      });

      return fo;
    },
    existsValue() {
      return this.value.length ? true : false;
    },
  },
  mounted() {
    this.fixTop();
  },
};
</script>

<style scoped>
.multiselect {
  background: #fff;
  padding: 1.4em;

  display: flex;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
  min-height: 33px;
  min-width: 222px;
  position: relative;
  flex-wrap: wrap;
  font-size: 12px;
}

.multiselect:focus {
  outline: none;
}
.multiselect__placeholder {
  outline: #929292;
}
.multiselect__selected {
  border: 1px solid #e0e0e0;
  padding: 4px 8px;
  padding-right: 0;
  margin: 3px 3px;
}

.multiselect__remove {
  cursor: pointer;
  padding-right: 7px;
}
.multiselect__remove:hover {
  cursor: red;
  font-weight: bold;
}

.multiselect__options {
  position: absolute;
  top: 34px;
  right: 0;
  left: 0;
  display: flex;
  background: #fff;
  flex-direction: column;
  box-shadow: 0 3px 3px 2px #e3e3e3;
  padding: 5px 0;
  min-height: 55px;
  max-height: 188px;
  overflow-y: auto;
}

.multiselect__option {
  padding: 6px 11px;
  cursor: pointer;
  display: flex;
  align-items: center;
}
.multiselect__option--checked {
  color: #1f7bb8;
  font-weight: bold;
}

.multiselect__checkbox {
  width: 22px;
  height: 22px;
  border: 1px solid #969696;
  margin-right: 7px;
  position: relative;
}
.multiselect__option--checked .multiselect__checkbox {
  border: 1px solid #1f7bb8;
  background: #1f7bb8;
}

.multiselect__option--checked .multiselect__checkbox::after {
  width: 11px;
  height: 6px;
  border-left: 2px solid rgb(255, 255, 255);
  border-bottom: 2px solid rgb(255, 255, 255);
  content: "";
  z-index: 9999;
  position: absolute;
  transform: rotate(-45deg);
  left: 3px;
  top: 4px;
}
</style>
