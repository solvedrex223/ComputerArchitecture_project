<template>
    <Line id="my-chart-id" :options="chartOptions" :data="chartData" ref="chart_ref"/>
</template>

<script setup lang="ts">
    import { CategoryScale, Chart as ChartJS, Legend, LineElement, LinearScale, PointElement, Title, Tooltip } from 'chart.js';
import { computed, defineComponent } from 'vue';
import { Line } from 'vue-chartjs';

    ChartJS.register(Title, Tooltip, Legend, LineElement, PointElement, CategoryScale, LinearScale)
    defineComponent({
        Line,
    });
    const props = defineProps({
        dataSets: {
            type: Array<number>,
            required: true
        },
        labels: {
            type: Array<string>,
            required: true
        }
    });

    const chartData = computed(() => {
        return {
            datasets: [{
                data:[...props.dataSets],
                label: 'Test',
                borderColor: '#f87979',
                backgroundColor: 'rgba(255, 99, 132, 0.2)',
                fill: true
            }],
            labels:[...props.labels]
        }
    });
    const chartOptions = {
        responsive: true
    }
</script>