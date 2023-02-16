# Repo for reproduction issue in vuejs 

## Issue 

When i try to create a component with v-model support. 
I meet some issue with reactivity. 
I keep only the necessary stuf to reproduce my issue.

```ts
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

function add(item: object) {
  selected.value.push(item);
}
```

In the code above, i try to manipulate my modelValue to filter some data (i removed the unecesary code). But when i use push on add() nothing append. when i comment the filter or the map it's work as expected ...

i can see that the filter return [proxy] and modelValue.value return proxy with array in target. But i don't know why ... Or how to work with.