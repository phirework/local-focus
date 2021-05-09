<template>
  <select @change="emitColour">
    <option value=""></option>
    <option :selected="selected === 'green'" value="green">green</option>
    <option :selected="selected === 'yellow'" value="yellow">yellow</option>
    <option :selected="selected === 'red'" value="red">red</option>
  </select>
</template>

<script>
export default {
  name: 'Colour',
  data: function() {
    return {
      colour: ''
    }
  },
  props: {
    selected: String
  },
  methods: {
    emitColour(e) {
      this.$emit('change-colour', e.target.value);
    }
  }
}
</script>

<style >
</style>
