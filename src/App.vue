<template>
  <div v-if="rate > 0">
    <h1>
      {{ result }}
    </h1>
    <h3>Vrijedi za: {{ this.validOn }}</h3>
    <div>
      <input
        id="input"
        type="number"
        v-model.number="inputValue"
        placeholder="Upiši iznos u kunama..."
        @keyup.enter="changeCurrency(inputValue)"
      />
    </div>
    <Button title="pretvori" @click="changeCurrency(inputValue)" />
  </div>
</template>

<script>
import Button from "./components/Button.vue";

function formatCurrency(value) {
  const replacedCommasWithDots = value.replace(/,/g, ".");
  const stringToNumber = parseFloat(replacedCommasWithDots);
  const roundNum = stringToNumber.toFixed(2);
  return roundNum;
}

export default {
  name: "App",
  components: {
    Button,
  },
  data() {
    return {
      rate: 0,
      valueHRK: 0,
      valueEUR: 0,
      showResult: false,
      validOn: "",
      inputValue: "",
    };
  },
  created() {
    fetch("/tecajn/v1?valuta=EUR")
      .then((res) => res.json())
      .then(([data]) => {
        this.rate = formatCurrency(data["Srednji za devize"]);
        this.validOn = data["Datum primjene"];
      })
      .catch((err) => console.log(err.message));
  },

  computed: {
    result() {
      if (this.inputValue === "" || this.valueHRK === 0) {
        return `1 HRK = ${this.rate} EUR`;
      }

      return `${this.valueHRK} HRK = ${this.valueEUR} EUR`;
    }
  },

  methods: {
    changeCurrency() {
      this.valueHRK = this.inputValue;
      this.valueEUR = (this.inputValue / this.rate).toFixed(2);
    },
  },
};
</script>

<style>
#title {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: black;
  font-size: 10px;
  margin-top: 30px;
  margin-left: 30px;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 30px;
}

#input {
  height: 40px;
  min-width: 100px;
  background-color: rgb(58, 56, 56);
  color: white;
  border: 0;
  border-radius: 4px;
  margin: 20px;
  width: 300px;
}
</style>
