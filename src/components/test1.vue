<script lang="ts" setup>
import { computed, defineEmits, defineProps, toRefs } from "vue";

type myObject = object & { [index: string]: any };
const props = defineProps<{
  referenceArray: myObject[];
  modelValue: myObject[];
  keyLabel: string;
}>();

const emit = defineEmits(["update:modelValue"]);

const { modelValue, referenceArray, keyLabel } = toRefs(props);

const selected = computed<any[]>({
  get: () => {
    const res = modelValue.value
      // .filter((x: myObject) => true) // push don't trigger change with this
      // .map(x => x) // or this
      .sort((a: myObject, b: myObject) =>
        a[keyLabel.value].localeCompare(b[keyLabel.value])
      ); // but work with this

    return res;
  },
  set: (value: any) => emit("update:modelValue", value),
});

const unselected = computed(() =>
  referenceArray.value.filter(
    (reference: myObject) =>
      !selected.value
        .map((x: any) => x[keyLabel.value])
        .includes(reference[keyLabel.value])
  )
);

function add(item: object) {
  selected.value.push(item);
}

function remove(item: object) {
  selected.value = selected.value.filter((s) => s !== item);
}
</script>

<template>
  <div class="flex flex-row gap-10">
    <div>
      <h1>Unselect</h1>
      <div class="flex flex-col gap-2">
        <div
          v-for="(item, index) in unselected"
          :key="index"
          @click="add(item)"
          class="cursor-pointer bg-gray-200 rounded hover:bg-gray-400 p-2"
        >
          {{ item }}
        </div>
      </div>
    </div>
    <div>
      <h1>Select</h1>
      <div class="flex flex-col gap-2">
        <div
          v-for="(item, index) in selected"
          :key="index"
          @click="remove(item)"
          class="cursor-pointer bg-gray-200 rounded hover:bg-gray-400 p-2"
        >
          {{ item }}
        </div>
      </div>
    </div>
  </div>
</template>
