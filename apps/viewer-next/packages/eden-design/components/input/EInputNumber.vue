<script lang="ts" setup>
import type { InputNumberProps } from "../types/EdenInput/InputNumberProps";
import { parseSize } from "../../_utils/styleUtils";
import { isNumber, toNumber } from "../../_utils/numberUtils";
import EInput from "./EInput.vue";
import EIconSubtract from "../icon/EIconSubtract.vue";
import EIconAdd from "../icon/EIconAdd.vue";

const props = withDefaults(defineProps<InputNumberProps>(), {
  align: "center",
  value: 0,
  min: 0,
  max: 100,
  step: 1,
  disabled: false,
  readonly: false,
  width: "auto",
  size: "medium",
});

const model = defineModel<number>();

watch(
  () => props.value,
  newVal => {
    if (isNumber(newVal)) {
      model.value = newVal;
    }
  }
);

watch(
  () => model.value,
  (oldVal, newVal) => {
    if (!newVal || !isNumber(newVal) || isNaN(newVal)) {
      model.value = oldVal;
      return;
    }
    if (newVal > props.max) {
      model.value = props.max;
    }
    if (newVal < props.min) {
      model.value = props.min;
    }
    emits("update:value", newVal);
  },
  { immediate: true }
);

const emits = defineEmits<{
  (e: "update:value", value: number): void;
}>();

const handleInput = () => {
  emits("update:value", toNumber(model.value));
};

const inputClass = computed(() => {
  return [
    "eden-ui eden-ui__input-number flex items-center",
    `size-${props.size}`,
    "transition-all duration-300 ease-in-out",
    {
      disabled: props.disabled,
      "!cursor-not-allowed": props.disabled,
    },
  ];
});

const inputStyle = computed(() => {
  return {
    width: parseSize(props.width),
  };
});

function handleAdd() {
  if (props.disabled) return;
  // @ts-ignore
  if (model.value + props.step <= props.max) {
    // @ts-ignore
    model.value = toNumber(model.value) + props.step;
  }
}

function handleSubtract() {
  if (props.disabled) return;
  // @ts-ignore
  if (model.value - props.step >= props.min) {
    // @ts-ignore
    model.value = toNumber(model.value) - props.step;
  }
}
</script>

<template>
  <e-input
    type="number"
    :align="align"
    :disabled="disabled"
    :class="inputClass"
    :style="inputStyle"
    @input="handleInput"
    v-model="model"
    :min="min"
    :max="max"
    :step="step"
  >
    <template #prefix>
      <span
        class="cursor-pointer h-full lh-0"
        @click="handleSubtract"
        :class="{
          '!cursor-not-allowed': disabled,
        }"
      >
        <e-icon-subtract size="20" />
      </span>
    </template>

    <template #suffix>
      <span
        class="cursor-pointer h-full lh-0"
        @click="handleAdd"
        :class="{
          '!cursor-not-allowed': disabled,
        }"
      >
        <e-icon-add size="20" />
      </span>
    </template>
  </e-input>
</template>

<style scoped lang="scss"></style>
