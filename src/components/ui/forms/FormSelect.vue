<template>
  <select v-model="SelectedValue" @change="changeOptions">
    <option value="">{{default_value??'Выберите из списка'}}</option>
    <option v-for="option in options" :key="option.id" :value="option.id">{{option.name}}</option>
  </select>
</template>

<script>
export default {
  name: "FormSelect",
  data(){
    return {SelectedValue:''}
  },
  props: {
    options: {
      type: Array,
      default: () => []
    },
    default_value: {
      type: String
    }
  },
  methods: {
    changeOptions(){
      this.$emit('update:modelValue', this.SelectedValue)
    }
  }
}
</script>

<style scoped>

</style>