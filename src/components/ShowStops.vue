<script setup lang="ts">
import { ref } from "vue";
import { busLineId } from "../dataObjs/arrays.ts";
let dataGot = ref<string>("");
async function fetchData(lineName: number) {
    const data = await fetch(
        `https://rozklady.mzk-konin.com.pl/getDirection.json?lineId=${busLineId[lineName]}&thereDirection=false`
    );
    const dataJson = await data.json();
    return dataJson;
}

function displayData(lineName: number = 51) {
    fetchData(lineName).then((data) => {
        let stopPointsArr: string[] = [];
        for (stop of data.stopPoints) {
            stopPointsArr.push(stop.name);
        }
        dataGot.value = stopPointsArr.join(", ");
    });
}

displayData("50");
</script>

<template>
    <select name="" id="" @change="displayData($event.target.value)">
        <option v-for="(line, index) in busLineId" :key="index" :value="index">
            {{ index }}
        </option>
    </select>
    <p>Przystanki: {{ dataGot }}</p>
</template>

<style scoped></style>
