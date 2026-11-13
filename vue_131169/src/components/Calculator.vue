<script setup lang="ts">
import { ref } from 'vue'

const display = ref('0')
const previousValue = ref<number | null>(null)
const operator = ref<string | null>(null)
const waitingForNextValue = ref(false)

const appendNumber = (num: string) => {
    if (waitingForNextValue.value) {
        display.value = num
        waitingForNextValue.value = false
    } else {
        display.value = display.value === '0' ? num : display.value + num
    }
}

const addDot = () => {
    if (waitingForNextValue.value) {
        display.value = '0.'
        waitingForNextValue.value = false
        return
    }
    if (!display.value.includes('.')) {
        display.value += '.'
    }
}

const clear = () => {
    display.value = '0'
    previousValue.value = null
    operator.value = null
    waitingForNextValue.value = false
}

const setOperator = (op: string) => {
    if (previousValue.value !== null && !waitingForNextValue.value) {
        calculate()
    }
    previousValue.value = parseFloat(display.value)
    operator.value = op
    waitingForNextValue.value = true
}

const calculate = () => {
    if (previousValue.value === null || operator.value === null) return

    const current = parseFloat(display.value)
    let result = 0

    switch (operator.value) {
        case '+':
            result = previousValue.value + current
            break
        case '-':
            result = previousValue.value - current
            break
        case '*':
            result = previousValue.value * current
            break
        case '/':
            result = current !== 0 ? previousValue.value / current : 0
            break
    }

    display.value = String(result)
    previousValue.value = null
    operator.value = null
    waitingForNextValue.value = true
}
</script>

<template>
    <div class="calculator-container">
        <h2>เครื่องคิดเลข</h2>
        <div class="calculator">
            <div class="display">{{ display }}</div>

            <div class="buttons">
                <button class="btn btn-clear" @click="clear">C</button>
                <button class="btn btn-op" @click="setOperator('/')">÷</button>
                <button class="btn btn-op" @click="setOperator('*')">×</button>
                <button class="btn btn-op" @click="setOperator('-')">-</button>

                <button class="btn" @click="appendNumber('7')">7</button>
                <button class="btn" @click="appendNumber('8')">8</button>
                <button class="btn" @click="appendNumber('9')">9</button>
                <button class="btn btn-op" @click="setOperator('+')">+</button>

                <button class="btn" @click="appendNumber('4')">4</button>
                <button class="btn" @click="appendNumber('5')">5</button>
                <button class="btn" @click="appendNumber('6')">6</button>
                <button class="btn btn-equals" @click="calculate">=</button>

                <button class="btn" @click="appendNumber('1')">1</button>
                <button class="btn" @click="appendNumber('2')">2</button>
                <button class="btn" @click="appendNumber('3')">3</button>
                <button class="btn zero" @click="appendNumber('0')">0</button>

                <button class="btn" @click="addDot">.</button>
            </div>
        </div>
    </div>
</template>

<style scoped>
.calculator-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-top: 1rem;
}

.calculator {
    width: 260px;
    background-color: #222;
    border-radius: 12px;
    padding: 16px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
}

.display {
    background-color: #333;
    color: #fff;
    font-size: 2rem;
    text-align: right;
    padding: 12px 16px;
    border-radius: 8px;
    margin-bottom: 14px;
    word-wrap: break-word;
}

.buttons {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 8px;
}

.btn {
    padding: 14px;
    font-size: 1.2rem;
    border: none;
    border-radius: 6px;
    background-color: #444;
    color: white;
    cursor: pointer;
    transition: background-color 0.2s;
}

.btn:hover {
    background-color: #555;
}

.btn-op {
    background-color: #ff9500;
}

.btn-op:hover {
    background-color: #e08400;
}

.btn-clear {
    background-color: #a5a5a5;
    color: black;
}

.btn-clear:hover {
    background-color: #8e8e8e;
}

.btn-equals {
    grid-row: span 2;
    background-color: #ff9500;
}

.zero {
    grid-column: span 2;
}
</style>