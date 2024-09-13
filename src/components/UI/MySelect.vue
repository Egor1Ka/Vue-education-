<template>
  <select :value="modelValue" @change="optionHandler">
    <option> Выберите из Списка </option>
    <option 
        v-for="option in options" 
        :key="option.value" 
        :value="option.value"> 
      {{ option.name }} 
    </option>
  </select>
</template>

<script>
export default {
  name: 'MySelect',
  props: {
    options: {
      type: Array,
      required: true,
    },
    modelValue: {
      type: String,
      required: true,
    }
  },
  methods: {
    optionHandler(event) {
      this.$emit('update:modelValue', event.target.value);
    }
  }
}
</script>

<style>
/* Ваши стили */
</style>
