<script setup>
    import { reactive, watch } from "vue";

    const state = reactive({
        num1: 0,
        num2: 0,
        operation: "add",
        result: 0,
        errorMessage: null,
    });

    const isValidNumber = (num) => {
        return num !== null && num !== "";
    };

    const calculate = () => {
        state.errorMessage = null;

        if (!isValidNumber(state.num1) || !isValidNumber(state.num2)) {
            state.result = 0;
            state.errorMessage = "Entrada inválida";
            return;
        }

        switch (state.operation) {
            case "add":
                state.result = formatNumber(
                    parseFloat(state.num1) + parseFloat(state.num2)
                );
                break;
            case "subtract":
                state.result = formatNumber(
                    parseFloat(state.num1) - parseFloat(state.num2)
                );
                break;
            case "multiply":
                state.result = formatNumber(
                    parseFloat(state.num1) * parseFloat(state.num2)
                );
                break;
            case "divide":
                if (state.num2 == 0) {
                    state.errorMessage = "Divisão por zero";
                    state.result = 0;
                } else {
                    state.result = formatNumber(
                        parseFloat(state.num1) / parseFloat(state.num2)
                    );
                }
                break;
        }
    };

    watch(
        [() => state.num1, () => state.num2, () => state.operation],
        calculate
    );

    function formatNumber(value) {
        const [whole, decimal] = value.toString().split(".");
        if (!decimal) return value;

        const digits = Math.min(decimal.length, 5);

        return parseFloat(value.toFixed(digits));
    }
</script>

<template>
    <div class="container text-center mt-5 p-4 rounded-5">
        <h1 class="title mb-4 text-light fw-bold">Calculadora</h1>
        <div class="mb-3">
            <input
                placeholder="Digite o primeiro número"
                type="number"
                v-model="state.num1"
                class="input form-control bg-dark-subtle" />
        </div>
        <div class="mb-3">
            <input
                placeholder="Digite o segundo número"
                type="number"
                v-model="state.num2"
                class="input form-control bg-dark-subtle" />
        </div>
        <div class="mb-3">
            <select
                v-model="state.operation"
                class="operator form-control fw-bold">
                <option class="text-center fw-bold" value="add">Soma</option>
                <option class="text-center fw-bold" value="subtract">
                    Subtração
                </option>
                <option class="text-center fw-bold" value="multiply">
                    Multiplicação
                </option>
                <option class="text-center fw-bold" value="divide">
                    Divisão
                </option>
            </select>
        </div>
        <p class="result lead text-light fw-bold">
            Resultado: {{ state.result }}
        </p>
        <p
            v-if="state.errorMessage"
            class="alert alert-danger text-danger fw-bold mt-3">
            {{ state.errorMessage }}
        </p>
    </div>
</template>

<style scoped>
    * {
        font-family: "Montserrat", sans-serif;
    }

    input[type="number"]::-webkit-outer-spin-button,
    input[type="number"]::-webkit-inner-spin-button {
        -webkit-appearance: none;
        margin: 0;
    }

    input[type="number"] {
        -moz-appearance: textfield;
        appearance: textfield;
    }
    .container {
        max-width: 100%;
        width: 300px;
        text-align: center;
        background-color: #646464;
    }

    .title {
        letter-spacing: 1px;
        text-transform: uppercase;
        font-size: 1.8em;
    }

    .input {
        text-align: center;
        font-weight: bold;
    }

    .operator,
    .operator:focus {
        background-color: #db914c;
        border: none;
        color: white;
        cursor: pointer;
        letter-spacing: 1px;
    }

    .result {
        letter-spacing: 1px;
    }
</style>

