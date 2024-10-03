<template>
    <div>
        <h3 class="text-center my-4">{{ title }}</h3>

        <div class="overflow-auto">
            <div
                class="chart d-flex gap-3"
                :style="{
                    '--max': Math.abs(max),
                    '--min': Math.abs(min),
                }"
            >
                <div
                    v-for="col in data"
                    :key="col.name"
                    class="chart-col position-relative"
                    :style="{ '--quantity': col.quantity }"
                    :class="{ negative: col.quantity < 0 }"
                >
                    <div class="chart-col-inner w-100 position-absolute"></div>
                </div>
            </div>
            <div class="chart-names d-flex gap-3 mt-3">
                <div
                    v-for="col in data"
                    :key="col.name"
                    class="d-flex align-items-center justify-content-end"
                >
                    <div>{{ col.name }}</div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
    const props = defineProps(["data", "title"]);

    const { min, max } = props.data.reduce(
        (acc, val) => {
            acc.min = Math.min(acc.min, val.quantity);
            acc.max = Math.max(acc.max, val.quantity);
            return acc;
        },
        { min: Infinity, max: -Infinity }
    );

    /**
     * "Допустимо передавать из контроллера в шаблон только данные: значения quantity и name, все расчеты по разметке должен делать css"
     * Данный пункт мне не дался, не было видимо такой нужды раньше. Сделал по минимуму расчеты в скрипт файле
     */
</script>

<style>
    .chart {
        height: 60vh;
        padding-top: 12rem;
    }

    .chart-col {
        min-width: calc(100% / 12);
    }

    .chart-col-inner {
        bottom: 50%;
        top: initial;
        height: calc(((var(--quantity) * 50) / var(--max)) * 1%);
        background-color: var(--bs-danger);
    }

    .chart-col.negative .chart-col-inner {
        top: 50%;
        bottom: initial;
        height: calc(((var(--quantity) * -1 * 50) / var(--min)) * 1%);
        background-color: var(--bs-success);
    }

    .chart-names > div {
        min-width: calc(100% / 12);
        writing-mode: vertical-rl;
        text-orientation: sideways;
        transform: rotate(-180deg);
    }

    .chart-col-inner::before {
        counter-reset: variable var(--quantity);
        content: counter(variable);

        display: inline-block;
        writing-mode: vertical-rl;
        text-orientation: sideways;
        transform: rotate(-180deg) translateX(calc(-50% + 1.5rem));
        position: absolute;
        bottom: calc(100% + 1rem);
        left: 50%;
    }
</style>
