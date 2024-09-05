<template>
  <div class="calculator">
    <div class="display">
      <span :style="RemoveDisplay">{{ display }}</span>
      <span :class="[finalOperator ? 'final' : 'beforeFinal']">{{ finalResult }}</span>
    </div>
    <div class="buttons">
      <button v-for="button in buttons" :key="button" @click="handleClick(button)">
        {{ button }}
      </button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      display: "",
      finalResult: "",
      currentValue: "0",
      operator: null,
      finalOperator: false,
      previousValue: null,
      waitingForNewValue: false,
      buttons: ["7", "8", "9", "÷", "4", "5", "6", "×", "1", "2", "3", "-", "0", ".", "=", "+", "C"],
    };
  },
  computed: {
    RemoveDisplay() {
      return {
        display: this.finalOperator ? "none" : "block",
      };
    },
  },
  watch: {
    finalOperator(newVal, oldVal) {
      console.log(newVal, oldVal);
    },
  },
  methods: {
    handleClick(button) {
      if (["+", "-", "×", "÷"].includes(button)) {
        this.operatorClick(button);
      } else if (button === "=") {
        this.calculate();
        this.clear();
        if (this.finalResult != "") {
          this.finalOperator = true;
        }
      } else if (button === "C") {
        this.clear();
        this.finalOperator = false;
      } else {
        if (this.finalOperator) {
          this.finalResult = "";
          this.clear();
        }
        this.numberClick(button);
        this.finalOperator = false;
      }
    },
    numberClick(number) {
      if (this.waitingForNewValue) {
        this.currentValue = number;
        this.display += number;
        this.waitingForNewValue = false;
      } else {
        if (this.currentValue === "0") {
          this.currentValue = number;
          this.display += number;
        } else {
          this.currentValue += number;
          this.display += number;
        }
      }
    },
    operatorClick(operator) {
      if (!this.waitingForNewValue) {
        this.calculate();
        this.previousValue = this.currentValue;
        // this.previousValue = this.display;
      }
      this.operator = operator;
      this.waitingForNewValue = true;
      this.display += operator;
    },
    calculate() {
      let result = parseFloat(this.previousValue);
      const current = parseFloat(this.currentValue);

      if (isNaN(result) || isNaN(current)) {
        return;
      }

      switch (this.operator) {
        case "+":
          result += current;
          break;
        case "-":
          result -= current;
          break;
        case "×":
          result *= current;
          break;
        case "÷":
          if (current !== 0) {
            result /= current;
          } else {
            result = "Error";
          }
          break;
      }

      this.currentValue = result.toString();
      this.finalResult = result.toString();
      this.operator = null;
      this.previousValue = null;
    },
    clear() {
      this.display = "";
      this.currentValue = 0;
      this.previousValue = null;
      this.operator = null;
      this.finalOperator = false;
    },
  },
};
</script>

<style scoped>
.calculator {
  display: flex;
  flex-direction: column;
  width: 300px;
  margin: 100px auto;
  box-shadow: 0px 10px 30px rgba(0, 0, 0, 0.1);
  border-radius: 10px;
  overflow: hidden;
}

.display {
  display: flex;
  flex-direction: column;
  background-color: #333;
  color: #fff;
  font-size: 2rem;
  padding: 20px;
  text-align: right;
}

.display .beforeFinal {
  font-size: 1rem;
  color: #bbb;
  margin-top: 0.5rem;
}

.display .final {
  color: #fff;
  font-size: 2rem;
}

.buttons {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-gap: 1px;
  background-color: #ccc;
}

button {
  background-color: #fff;
  border: none;
  padding: 20px;
  font-size: 1.5rem;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #f0f0f0;
}

button:active {
  background-color: #ddd;
}
</style>
