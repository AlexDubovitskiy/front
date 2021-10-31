<script>
export default {
  name : "ui-select",
  props: ['list'],
  watch: {
    value(newValue) {
      this.$emit('onchange', newValue)
      this.$emit('callback');
    }
  },
  data() {
    return {
      value: undefined
    }
  },
}
</script>

<template>
  <select v-model="value" class = "form-control">
    <option v-for="item of list">{{item}}</option>
  </select>
</template>
