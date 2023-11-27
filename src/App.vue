<template>
  <div id="app">
    <h1>Enter triangle sides:</h1>

    <label for="side_a">Size A </label>
    <input @keydown="clear" v-model="sideA" id="side_a" />
    <span v-if="sideA && !validation.a" class="error"
      >Must be a positive number, less than 10 characters.</span
    >

    <label for="side_b">Size B</label>
    <input @keydown="clear" v-model="sideB" id="side_b" />
    <span v-if="sideB && !validation.b" class="error"
      >Must be a positive number, less than 10 characters.</span
    >

    <label for="side_c">Size C</label>
    <input @keydown="clear" v-model="sideC" id="side_c" />
    <span v-if="sideC && !validation.c" class="error"
      >Must be a positive number, less than 10 characters.</span
    >

    <br />
    <br />

    <button
      @click="test"
      :disabled="!validation.a || !validation.b || !validation.c"
    >
      Test your numbers
    </button>

    <br />

    <div class="result" v-if="displayDots">Calculating ...</div>

    <div class="result" v-if="displayResults">
      <span class="yes" v-if="isTriangle"
        >"Yes, you can create a triangle"</span
      >
      <span class="no" v-else>"Sorry, you cannot create a triangle"</span>
    </div>

    <br />
    <br />

    <h3>History:</h3>
    <div class="history" v-for="(tr, index) in history" :key="index">
      Sides: {{ tr.a }} / {{ tr.b }} / {{ tr.c }} Result:
      {{ tr.result }}
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
  data() {
    return {
      sideA: "",
      sideB: "",
      sideC: "",
      isTriangle: false,
      displayResults: false,
      displayDots: false,
      history: [],
    };
  },
  computed: {
    validation() {
      return {
        a: this.sideA > 0 && !isNaN(this.sideA) && this.sideA.length < 10,
        b: this.sideB > 0 && !isNaN(this.sideB) && this.sideB.length < 10,
        c: this.sideC > 0 && !isNaN(this.sideC) && this.sideC.length < 10,
      };
    },
  },

  methods: {
    clear() {
      this.displayResults = false;
      this.displayDots = false;
    },
    test() {
      this.displayResults = false;
      this.displayDots = true;

      let sides = [];
      sides.push(parseInt(this.sideA));
      sides.push(parseInt(this.sideB));
      sides.push(parseInt(this.sideC));

      sides.sort(function (a, b) {
        return b - a;
      });

      // longest side [0]
      this.isTriangle = sides[1] + sides[2] > sides[0];

      setTimeout(() => {
        this.displayDots = false;
        this.displayResults = true;

        this.history.push({
          a: this.sideA,
          b: this.sideB,
          c: this.sideC,
          result: this.isTriangle ? "Yes" : "No",
        });
      }, 1500);
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  padding: 10px;
}
label {
  display: block;
}

input {
  width: 100%;
  max-width: 300px;
}
.result {
  margin-top: 20px;
}
.yes {
  color: green;
}
.no {
  color: red;
}
.error {
  display: block;
  font-size: 12px;
  color: red;
}
</style>
