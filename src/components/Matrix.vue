<template>
  <div class="container">
    <header>
      <h1 class="title">LED Matrix emulator</h1>
    </header>
    <article class="wrapper">
      <div class="matrix">
        <div>
          <square :input="squares[0]" />
          <square :input="squares[1]" />
          <square :input="squares[2]" />
          <square :input="squares[3]" />
        </div>
        <div>
          <square :input="squares[4]" />
          <square :input="squares[5]" />
          <square :input="squares[6]" />
          <square :input="squares[7]" />
        </div>
        <div>
          <square :input="squares[8]" />
          <square :input="squares[9]" />
          <square :input="squares[10]" />
          <square :input="squares[11]" />
        </div>
        <div>
          <square :input="squares[12]" />
          <square :input="squares[13]" />
          <square :input="squares[14]" />
          <square :input="squares[15]" />
        </div>
      </div>
    </article>
  </div>
</template>

<script>
import Square from './Square';
import _ from 'lodash';

import C from '../assets/characters';

Array.range = function(n) {
  return Array.apply(null, Array(n)).map((x, i) => i);
};

Object.defineProperty(Array.prototype, 'chunk', {
  value: function(n) {
    return Array.range(Math.ceil(this.length / n)).map((x, i) =>
      this.slice(i * n, i * n + n),
    );
  },
});

function text2dots(text) {
  const testArray = text.split('');

  const dots = _.flatten(
    testArray.map(char => C[char].dots.slice(0, C[char].width + 1)),
  );

  for (let i = 0; i < dots.length % 8; i++) {
    dots.push(0x00);
  }

  return dots;
}

export default {
  components: {
    Square,
  },

  computed: {
    text() {
      return this.$route.query.text;
    },
  },

  data() {
    return {
      squares: Array.from({ length: 16 }, () =>
        Array.from({ length: 8 }, () => 0x00),
      ),
    };
  },

  mounted() {
    if (!this.text) {
      return;
    }

    const text = text2dots(this.text);
    text.chunk(8).forEach((square, i) => {
      this.$set(this.squares, i, square);
    });
  },
};
</script>

<style scoped>
h1 {
  font-family: sans-serif;
}

.container {
  background-color: whitesmoke;
  height: 100vh;
}

header {
  display: flex;
}

.title {
  color: dimgray;
  margin: auto;
  padding: 1.25em;
}

.wrapper {
  display: flex;
}

.matrix {
  background-color: #202020;
  filter: drop-shadow(0 0 1em black);
  margin: auto;
}
</style>
