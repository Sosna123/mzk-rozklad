<script setup lang="ts">
import { ref } from "vue";
import { busLineId } from "../dataObjs/arrays.ts";
let stopsArr = ref<string[]>([]);
let currentLine = ref<number>(50);
let currentDirection = ref<boolean>(false);
let irreversibleLines = ["50", "52", "62", "100"];
async function fetchData(lineName: number, direction: boolean = false) {
    if (irreversibleLines.includes(lineName)) {
        direction = false;
    }
    const data = await fetch(
        `https://rozklady.mzk-konin.com.pl/getDirection.json?lineId=${busLineId[lineName]}&thereDirection=${direction}`
    );
    const dataJson = await data.json();
    return dataJson;
}

function displayData(lineName: number = 51, direction: boolean = false) {
    if (irreversibleLines.includes(lineName)) {
        direction = false;
    }
    currentLine.value = lineName;
    fetchData(lineName).then((data) => {
        stopsArr.value = [];
        for (stop of data.stopPoints) {
            stopsArr.value.push(stop.name);
        }
        if (direction) {
            stopsArr.value.reverse();
        }
    });
}

displayData("50");
</script>

<template>
    <select name="" id="" @change="displayData($event.target.value, false)">
        <option v-for="(line, index) in busLineId" :key="index" :value="index">
            {{ index }}
        </option>
    </select>
    <button
        @click="
            currentDirection = !currentDirection;
            displayData(currentLine, currentDirection);
        ">
        Change direction
    </button>
    <ul>
        <li v-for="stop in stopsArr">{{ stop }}</li>
    </ul>
</template>

<style scoped></style>
