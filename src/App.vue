<script lang="ts" setup>
import { ref } from "vue";
import test1 from "./components/test1.vue";

const items = [{ value: "test" }, { value: "hello" }, { value: "world" }];
const myModel = ref<object[]>([{ value: "hello" }, { value: "world" }]);
</script>

<template>
  <div class="flex flex-col gap-10">
    <test1 v-model="myModel" :reference-array="items" key-label="value" />
    <div>model = {{ myModel }}</div>
  </div>
</template>