<template>
    <v-container>
        <v-row>
            <v-col cols="12" md="6">
                <BarChart :dataSets="dataSets1" :labels="labels" label="Speed"/>
            </v-col>
            <v-col cols="12" md="6">
                <BarChart :dataSets="dataSets2" :labels="labels" label="RPM"/>
            </v-col>
        </v-row>
        <v-row>
            <v-col cols="12" md="6">
                <BarChart :dataSets="dataSets3" :labels="labels" label="Tmp"/>
            </v-col>
            <v-col cols="12" md="6">
                <BarChart :dataSets="dataSets4" :labels="labels" label="Throttle"/>
            </v-col>
        </v-row>
        <v-btn @click.stop="test">Test</v-btn>

    </v-container>
</template>


<script setup lang="ts">
    import { ref } from 'vue';
    import mqtt from "mqtt";

    const labels = ref(['', '', '', '', '','','','','','']);
    const dataSets1 = ref([65, 59, 80, 81, 56]);
    const dataSets2 = ref([28, 48, 40, 19, 86]);
    const dataSets3 = ref([18, 48, 77, 9, 100]);
    const dataSets4 = ref([45, 25, 16, 36, 67]);

    function test() {
        const newValue = Math.floor(Math.random() * 100);
        dataSets1.value.push(newValue);
        labels.value.push('');
}

    const client = mqtt.connect("ws://192.168.1.4:9001");
    client.subscribe('car/speed');
    client.subscribe('car/rpm');
    client.subscribe('car/temp');
    client.subscribe('car/throttle');

    client.on("message", (topic, message) => {
        console.log(topic + ': ' + message);
        setValues(topic,message.toString());
    });

    function setValues(topic:string, message:string) {
        switch (topic) {
            case 'car/speed':
                if(dataSets1.value.length < 10){
                    dataSets1.value.push(Number(message));
                }
                else{
                    dataSets1.value.splice(0,1);
                    dataSets1.value.push(Number(message));
                }
                break;
            case 'car/rpm':
                if(dataSets2.value.length < 10){
                    dataSets2.value.push(Number(message));
                }
                else{
                    dataSets2.value.splice(0,1);
                    dataSets2.value.push(Number(message));
                }
                break;
            case 'car/temp':
                if(dataSets3.value.length < 10){
                    dataSets3.value.push(Number(message));
                }
                else{
                    dataSets3.value.splice(0,1);
                    dataSets3.value.push(Number(message));
                }
                break;
            case 'car/throttle':
                if(dataSets4.value.length < 10){
                    dataSets4.value.push(Number(message));
                }
                else{
                    dataSets4.value.splice(0,1);
                    dataSets4.value.push(Number(message));
                }
                break;
        }
    }
</script>