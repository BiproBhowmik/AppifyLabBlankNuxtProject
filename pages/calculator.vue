<template>
  <div>
    <input
      type="number"
      v-model="inputData"
      placeholder="Input number"
      min="0"
    />
    <button @click="add()">+</button>
    <button @click="sub()">-</button>
    <button @click="mul()">*</button>
    <button @click="div()">/</button>
    <button @click="reset()">Reset</button>

    <div>Total = {{ total }} <button @click="rootOver()">√</button></div>
    <div>
      Total *
      <input
        type="number"
        v-model="percentData"
        placeholder="Input number"
        min="0"
      />
      <button @click="percent()">%</button>
    </div>
    <div>{{ totalString }}</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      total: 0,
      inputData: 0,
      percentData: 0,
      totalString: ""
    };
  },
  methods: {
    add() {
      this.total += parseFloat(this.inputData);
      this.totalString += " + " + this.inputData;
    },

    sub() {
      this.total -= parseFloat(this.inputData);
      this.totalString += " - " + this.inputData;
    },
    mul() {
      this.total *= parseFloat(this.inputData);
      this.totalString += " * " + this.inputData;
    },
    div() {
      if (this.inputData != 0) {
        this.total /= parseFloat(this.inputData);
        this.totalString += " / " + this.inputData;
      }
    },
    percent() {
      this.total *= parseFloat(this.percentData) / 100;
      this.totalString += " % " + this.inputData;
    },
    rootOver() {
      this.total = Math.sqrt(this.total);
      this.totalString += " √(Total) ";
    },
    reset() {
      this.total = 0;
      this.inputData = 0;
      this.percentData = 0;
      this.totalString = "";
    }
  }
};
</script>
