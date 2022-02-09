<template>
  <button @click="click">{{ title }}</button>
</template>

<script>
export default {
  name: "Button",
  props: ["title", "click"],
};
</script>

<style scoped>
button {
  height: 40px;
  min-width: 150px;
  background: rgb(25, 172, 115);
  color: white;
  border: 0;
  border-radius: 4px;
  text-transform: uppercase;
}

button:hover {
  background: rgb(171, 182, 125);
  cursor: pointer;
}
</style>
