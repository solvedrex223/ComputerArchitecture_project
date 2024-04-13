<template>
    <BarChart :dataSets="datasets" :labels="labels"/>
    <v-btn @click.stop="test">Test</v-btn>
</template>

<script setup lang="ts">
    import { ref } from 'vue';

    const labels = ref([ '', '', '' ]);
    const datasets= ref([40, 20, 12]);
    function test(){
        datasets.value.push(50);
        labels.value.push('');
    }
</script>