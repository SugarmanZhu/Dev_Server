<script setup lang="ts">

let start : number = Date.now();
let waitTime = randomNumber(1000, 3000);
let state : number = -1
let test = document.createElement('test')
let message = document.createElement('message');
const app = document.getElementById("app");
let reactionTime : number = 0;
let cnt = 0;

function randomNumber(min : number, max : number) { // min and max included 
    return Math.floor(Math.random() * (max - min + 1) + min)
}

async function delay(ms: number, this_cnt: number) {
    await new Promise( resolve => setTimeout(resolve, ms) );
    if (state == 1 && this_cnt == cnt) {
        test.style.backgroundColor = "rgb(65, 218, 115)";
    }
}

function update() {
    state = (state + 1) % 3;
    if (state == 0) {
        test.style.backgroundColor = "rgb(52, 137, 205)";
        message.innerText = "Click to start";
    } else if (state == 1) {
        test.style.backgroundColor = "rgb(206, 38, 56)";
        message.innerText = "Wait for green";
        waitTime = randomNumber(1000, 3000);
        start = Date.now();
        delay(waitTime, cnt);
    } else if (state == 2) {
        reactionTime = Date.now() - start - waitTime;
        test.style.backgroundColor = "rgb(52, 137, 205)";
        if (reactionTime < 0) {
            message.innerText = "You clicked too fast";
            cnt++;
        } else {
            message.innerText = "Reaction time: " + reactionTime + " ms";
        }
    } else {
        console.log("Invalid State");
    }
}

test.addEventListener('mousedown', update);
if (app) {
    app.appendChild(test);
}
test.appendChild(message);
update();

</script>

<template>

</template>

<style scoped>
</style>