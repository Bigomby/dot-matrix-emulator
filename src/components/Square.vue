<template>
  <div class="square">
    <div v-for="dot in square" :class="[ dot > 0 ? 'dot-on' : 'dot-off' ]" />
  </div>
</template>

<script>
import _ from 'lodash';

export default {
  props: {
    input: {
      type: Array,
      default: () => [0, 0, 0, 0, 0, 0, 0, 0],
    },
  },

  computed: {
    square() {
      const square = this.input
        .map(column => column.toString(2))
        .map(column => column.padStart(8, '0'))
        .map(column => column.split(''));

      return _.flatten(_.unzip(square));
    },
  },

  mounted() {},
};
</script>

<style scoped>
.square {
  transform: scaleY(-1);
  border: 1px solid dimgray;
  display: inline-grid;
  width: 12em;
  height: 12em;
  grid-template-columns: repeat(8, 1fr);
  align-items: center;
  grid-column-gap: 0.5em;
  grid-row-gap: 0.5em;
  padding: 0.25em;
}

.dot-on {
  background-color: red;
  width: 100%;
  height: 100%;
  border-radius: 50%;
}

.dot-off {
  background-color: dimgray;
  width: 100%;
  height: 100%;
  border-radius: 50%;
}
</style>
