<template>
  <input class="input"
         :value="modelValue"
         @input="$emit('update:modelValue', $event.target.value)"
  >
</template>

<script>
export default {
  name: "MyInput",
  props: {
    modelValue: String
  }
}
</script>

<style scoped>
.input {
  display: block;
  border: none;
  outline: none;
  width: 100%;
}

</style>