<template>
    <div>
        <h3 class="text-center my-4">{{ title }}</h3>

        <div class="overflow-x-auto">
            <div
                class="chart d-flex gap-3 justify-content-center"
                :style="{
                    '--amplitude': amplitude,
                    '--max': max,
                    '--min': min,
                }"
            >
                <div
                    v-for="col in data"
                    :key="col.name"
                    class="chart-col position-relative"
                    :style="{ '--quantity': col.quantity }"
                    :class="{ negative: col.quantity < 0 }"
                >
                    <div class="chart-col-bar w-100 position-absolute"></div>
                    <div
                        class="chart-col-name w-100 h-100 d-flex align-items-center justify-content-end position-absolute border-bottom border-black pb-2"
                    >
                        <div class="chart-col-name-inner">{{ col.name }}</div>
                    </div>
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
        { min: 0, max: 0 }
    );

    const amplitude = Math.abs(min - max);

    /**
     * "Допустимо передавать из контроллера в шаблон только данные: значения quantity и name, все расчеты по разметке должен делать css"
     * Данный пункт мне не дался, не было видимо такой нужды раньше. Сделал по минимуму расчеты в скрипт файле
     */
</script>

<style>
    .chart {
        height: 50rem;
        padding-top: 12rem;
        padding-bottom: 20rem;
    }

    .chart-col {
        min-width: 4rem;
    }

    .chart-col-bar {
        bottom: calc(((var(--min) * -1 * 100) / var(--amplitude)) * 1%);
        height: calc(((var(--quantity) * 100) / var(--amplitude)) * 1%);
        top: initial;
        background-color: var(--bs-danger);
    }

    .chart-col.negative .chart-col-bar {
        top: calc(((var(--max) * 100) / var(--amplitude)) * 1%);
        bottom: initial;
        height: calc(((var(--quantity) * -1 * 100) / var(--amplitude)) * 1%);
        background-color: var(--bs-success);
    }

    .chart-col-bar::before {
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

    .chart-col-name {
        top: calc(100% + 1rem);
        writing-mode: vertical-rl;
        text-orientation: sideways;
        transform: rotate(-180deg);
    }

    .chart-col-name-inner {
        transform: rotate(10deg);
        transform-origin: bottom;
    }
</style>
