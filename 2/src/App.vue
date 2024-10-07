<script setup>
    import Diagram from "@/components/Diagram.vue";
    import data from "../diagram-data-second-task.json";

    const collection = Object.keys(data).map((key) => ({
        title: key,
        data: data[key].toSorted((a, b) => a.quantity - b.quantity),
    }));
</script>

<template>
    <div class="container">
        <h1 class="text-center my-5">Диаграммы</h1>
        <template v-if="collection.length">
            <Diagram
                v-for="(dataset, index) in collection"
                :key="index"
                :title="dataset.title"
                :data="dataset.data"
                class="mt-4"
            />
        </template>
    </div>
</template>
