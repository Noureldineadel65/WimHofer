<script>
  import { onMount, createEventDispatcher, onDestroy } from "svelte";
  import Rhombus from "./Rhombus.svelte";
  import { scale } from "svelte/transition";
  const dispatch = createEventDispatcher();
  let scaled = false;
  let displayBreath = false;
  let breathCount = 0;
  let round = 1;
  let breathingInterval;

  const time = 3500;
  let audio;
  let start = false;
  let tone;
  const countDownTime = 3;
  let holdState = true;
  let countDown = 0;
  const messages = [
    "BRING AWARENESS TO YOUR BREATH",
    "BREATHE IN",
    "BREATHE OUT"
  ];
  let breatheState = "";
  onMount(() => {
    // audio.oncanplaythrough = function() {
    //   alert("wtf");
    // };
  });
  function startBreathing() {
    if (breathingInterval) return;

    audio.play();
    displayBreath = true;
    breathingInterval = setInterval(() => {
      if (!scaled) {
        breatheState = "BREATHE OUT";
      } else {
        breatheState = "BREATHE IN";
        breathCount++;
        // if (breathCount === 3) {
        //   clearInterval(breathingInterval);
        // }
      }
      scaled = !scaled;
    }, time);
  }
  function play() {
    start = true;
    tone.play();
    const ready = setInterval(() => {
      countDown++;
      if (countDown === countDownTime) {
        clearInterval(ready);
        tone.pause();
        startBreathing();
      }
    }, 1000);
  }
  onDestroy(() => {
    clearInterval(breathingInterval);
  });
</script>

<style>
  .breathe-state {
    text-align: center;
    font-size: 2rem;
    font-weight: bolder;
    display: block;
    left: 50%;
    transform: translateX(-50%);
    margin-top: -4rem;
  }

  .round {
    font-size: 2rem;
    position: absolute;
    transform: translateX(-50%);

    left: 50%;
  }
</style>

{#if start}
  <div class="round text-center" transition:scale>ROUND {round}</div>
{/if}
<div class="rhombus-container">
  <Rhombus
    {time}
    {displayBreath}
    {scaled}
    {start}
    {countDown}
    {breathCount}
    on:play={play} />
</div>
{#if displayBreath}
  <div class="breathe-state absolute" transition:scale>
    {breathCount === 0 && !breatheState ? messages[0] : breatheState}
  </div>
{/if}
<audio
  src="./meditation.mp3"
  id="audio"
  bind:this={audio}
  loop
  preload="auto" />
<audio src="./tone.mp3" id="tone" bind:this={tone} loop preload="auto" />
