<script setup lang="ts">
import { computed, onMounted, reactive, ref } from "vue";
import Calculator from "./Calculator.vue";
import Display from "./Display.vue";

interface Button {
  name: string;
  value: string;
  type: ButtonType;
  logicalValue: string | number;
}

const BUTTON_TYPES = {
  FUNCTION: "function",
  OPERATOR: "operator",
  NUMBER: "number",
} as const;

export type ObjectValues<T> = T[keyof T];

export type ButtonType = ObjectValues<typeof BUTTON_TYPES>;

const buttons = ref<Button[]>([
  {
    name: "clear",
    value: "C",
    type: BUTTON_TYPES.FUNCTION,
    logicalValue: "",
  },
  {
    name: "divide",
    value: "÷",
    type: BUTTON_TYPES.OPERATOR,
    logicalValue: "/",
  },
  {
    name: "multiply",
    value: "x",
    type: BUTTON_TYPES.OPERATOR,
    logicalValue: "*",
  },
  {
    name: "subtract",
    value: "-",
    type: BUTTON_TYPES.OPERATOR,
    logicalValue: "-",
  },
  {
    name: "add",
    value: "+",
    type: BUTTON_TYPES.OPERATOR,
    logicalValue: "+",
  },
  {
    name: "equals",
    value: "=",
    type: BUTTON_TYPES.FUNCTION,
    logicalValue: "",
  },
  {
    name: "zero",
    value: "0",
    type: BUTTON_TYPES.NUMBER,
    logicalValue: 0,
  },
  {
    name: "one",
    value: "1",
    type: BUTTON_TYPES.NUMBER,
    logicalValue: 1,
  },
  {
    name: "two",
    value: "2",
    type: BUTTON_TYPES.NUMBER,
    logicalValue: 2,
  },
  {
    name: "three",
    value: "3",
    type: BUTTON_TYPES.NUMBER,
    logicalValue: 3,
  },
  {
    name: "four",
    value: "4",
    type: BUTTON_TYPES.NUMBER,
    logicalValue: 4,
  },
  {
    name: "five",
    value: "5",
    type: BUTTON_TYPES.NUMBER,
    logicalValue: 5,
  },
  {
    name: "six",
    value: "6",
    type: BUTTON_TYPES.NUMBER,
    logicalValue: 6,
  },
  {
    name: "seven",
    value: "7",
    type: BUTTON_TYPES.NUMBER,
    logicalValue: 7,
  },
  {
    name: "eight",
    value: "8",
    type: BUTTON_TYPES.NUMBER,
    logicalValue: 8,
  },
  {
    name: "nine",
    value: "9",
    type: BUTTON_TYPES.NUMBER,
    logicalValue: 9,
  },
  {
    name: "decimal",
    value: ".",
    type: BUTTON_TYPES.NUMBER,
    logicalValue: ".",
  },
]);

const calculation = reactive({
  firstNumber: "",
  secondNumber: "",
  operator: "",
  result: null as number | null,
});

const handleButtonClick = (button: Button) => {
  switch (button.type) {
    case BUTTON_TYPES.FUNCTION:
      handleFunction(button);
      break;
    case BUTTON_TYPES.OPERATOR:
      handleOperator(button);
      break;
    case BUTTON_TYPES.NUMBER:
      handleNumber(button);
      break;
    default:
      break;
  }
};

const handleFunction = (button: Button) => {
  switch (button.name) {
    case "clear":
      clear();
      break;
    case "equals":
      calculate();
      break;
    default:
      break;
  }
};

const handleOperator = (button: Button) => {
  if (calculation.result) {
    calculation.firstNumber = calculation.result.toString();
    calculation.secondNumber = "";
    calculation.operator = "";
    calculation.result = null;
  }

  if (
    calculation.firstNumber &&
    calculation.secondNumber &&
    calculation.operator
  ) {
    const result = calculate();
    calculation.firstNumber = result.toString();
    calculation.secondNumber = "";
    calculation.operator = "";
    calculation.result = null;
  }

  if (typeof button.logicalValue === "string") {
    calculation.operator = button.logicalValue;
  }
};

const handleNumber = (button: Button) => {
  if (calculation.result) {
    clear();
  }

  if (calculation.operator) {
    calculation.secondNumber += button.logicalValue;
  } else {
    calculation.firstNumber += button.logicalValue;
  }
};

const clear = () => {
  calculation.firstNumber = "";
  calculation.secondNumber = "";
  calculation.operator = "";
  calculation.result = null;
};

const calculate = () => {
  if (
    calculation.firstNumber &&
    calculation.secondNumber &&
    calculation.operator
  ) {
    const firstNumber = Number(calculation.firstNumber);
    const secondNumber = Number(calculation.secondNumber);

    return (calculation.result = Function(
      `return ${firstNumber} ${calculation.operator} ${secondNumber}`
    )());
  }
};

const displayCalculation = computed(() => {
  return calculation.result
    ? calculation.result
    : `${calculation.firstNumber}  ${calculation.operator}  ${calculation.secondNumber}`;
});

onMounted(() => {
  window.addEventListener("keydown", (e) => {
    const button = buttons.value.find(
      (button) => button.logicalValue.toString() === e.key
    );

    if (button) {
      handleButtonClick(button);
    }

    if (e.key === "Enter") {
      calculate();
    }

    if (e.key === "c") {
      clear();
    }
  });
});
</script>
<template>
  <Calculator class="calculator">
    <Display class="h-56">
      <template #display>
        {{ displayCalculation }}
      </template>
    </Display>
    <div class="calculator__buttons flex-1 p-12">
      <div v-for="button in buttons" :class="button.name" class="w-full h-full">
        <button
          class="h-full w-full bg-[#1F2937] rounded-xl text-white text-4xl font-bold"
          @click="handleButtonClick(button)"
          :key="button.name"
        >
          {{ button.value }}
        </button>
      </div>
    </div>
  </Calculator>
</template>

<style lang="scss">
.calculator {
  &__buttons {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-template-rows: repeat(5, 1fr);
    grid-template-areas:
      "clear .     .       divide"
      "seven eight nine    multiply"
      "four  five  six     subtract"
      "one   two   three   add"
      "zero  .     decimal equals";
    gap: 1.5rem;
  }
}

.clear {
  grid-area: clear;
}

.divide {
  grid-area: divide;
}

.multiply {
  grid-area: multiply;
}

.subtract {
  grid-area: subtract;
}

.add {
  grid-area: add;
}

.equals {
  grid-area: equals;
}

.zero {
  grid-area: zero;
}

.one {
  grid-area: one;
}

.two {
  grid-area: two;
}

.three {
  grid-area: three;
}

.four {
  grid-area: four;
}

.five {
  grid-area: five;
}

.six {
  grid-area: six;
}

.seven {
  grid-area: seven;
}

.eight {
  grid-area: eight;
}

.nine {
  grid-area: nine;
}

.decimal {
  grid-area: decimal;
}
</style>
