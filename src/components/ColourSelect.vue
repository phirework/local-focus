<template>
  <select class="task-meta" @change="emitColour">
    <option :class="c" v-for="c in options" :selected="selected == c" :key="c" :value="c">✎</option>

  </select>
</template>

<script>
export default {
  name: 'Colour',
  data: function() {
    return {
      colour: '',
      options: [
        "green",
        "yellow",
        "red"
      ]
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

<style>
select {
  appearance: none;
  border: 0;
  background: transparent;
  font-size: 20px;
  padding-left: 40px;
  padding-top: 0;
  padding-bottom: 0;
  text-align: center;
  font-weight: bold;
}

</style>
