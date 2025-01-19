<script setup lang="ts">
import { ref } from "vue";
import { busLineId } from "./dataObjs/arrays.ts";
let dataGot = ref<string>("");
async function fetchData(lineName: number) {
    console.log(busLineId[lineName], lineName);
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

displayData("51");
</script>

<template>
    <select name="" id="" @change="displayData($event.target.value)">
        <option value="50">50</option>
        <option selected value="51">51</option>
    </select>
    <p>Przystanki: {{ dataGot }}</p>
</template>

<style scoped></style>
