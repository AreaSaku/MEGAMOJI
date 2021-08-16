<script lang="ts">
import { defineComponent } from "vue";
import ColorPopover from "./ColorPopover.vue";
import { HEX2HSL } from "../../utils/color";

export default defineComponent({
  components: {
    ColorPopover,
  },
  props: {
    modelValue: { type: String, required: true },
    block: { type: Boolean, default: false },
    style: { type: [String, Object], default: {} },
  },
  computed: {
    dark(): boolean {
      return HEX2HSL(this.modelValue).l < 50;
    },
  },
  data: (props) => ({
    showPopover: false,
  }),
  methods: {
    hidePopover(): void {
      this.showPopover = false;
    },
  },
});
</script>

<template>
  <button
      ref="input"
      :class="['color', { block, dark }]"
      :style="{ ...style, backgroundColor: modelValue }"
      @click="showPopover = !showPopover">
    {{ modelValue }}
  </button>
  <ColorPopover
      :model-value="modelValue"
      :show="showPopover"
      :el="$refs.input"
      :on-hide="hidePopover"
      @update:model-value="$emit('update:modelValue', $event)" />
</template>

<style scoped>
.color {
  display: inline-block;
  min-width: 90px;
  box-sizing: border-box;
  padding: var(--paddingMedium);
  font-size: var(--fontSizeMedium);
  line-height: 1;
  cursor: pointer;
  color: var(--fg);
  background-color: var(--bg);
  border: 1px solid var(--border);
  border-radius: var(--borderRadius);
}

.color.dark {
  color: var(--distantFg);
}

.color:hover {
  border-color: var(--primaryHover);
}

.color:active {
  border-color: var(--primaryActive);
  box-shadow: var(--primaryShadow);
}

.block {
  display: block;
  width: 100%;
}
</style>