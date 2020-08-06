<script>
  import { onMount, createEventDispatcher, onDestroy } from "svelte";
  import Rhombus from "./Rhombus.svelte";
  import Circle from "./Circle.svelte";
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
  let holdState = false;
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
    breatheState = "BRING AWARENESS TO YOUR BREATH";
    breathingInterval = setInterval(() => {
      if (!scaled) {
        breatheState = "BREATHE OUT";
      } else {
        breatheState = "BREATHE IN";
        breathCount++;
        if (breathCount === 1) {
          clearInterval(breathingInterval);
          finishBreathing();
        }
      }
      scaled = !scaled;
    }, time);
  }
  function finishBreathing() {
    displayBreath = false;
    start = false;
    breatheState = "HOLD YOUR BREATH";
    holdState = true;
    stopAudio(audio);
  }
  function play() {
    start = true;
    tone.play();
    const ready = setInterval(() => {
      countDown++;
      if (countDown === countDownTime) {
        clearInterval(ready);
        stopAudio(tone);
        startBreathing();
      }
    }, 1000);
  }
  function stopAudio(sound) {
    sound.pause();
    sound.currentTime = 0;
  }
  function holdEnd() {
    let scaled = false;
    let displayBreath = false;
    let breathingInterval;

    startBreathing();
    start = false;
    holdState = false;
    round++;
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
    /* margin-top: -4rem; */
    bottom: -23rem;
  }

  .round {
    font-size: 2rem;
    position: absolute;
    transform: translateX(-50%);
    top: -23rem;

    left: 50%;
  }
  .shape-container {
    width: 100%;
    height: 100%;
  }
  .rhombus-container,
  .circle-container {
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
</style>

{#if start}
  <div class="round text-center" transition:scale>ROUND {round}</div>
{/if}
<div class="shape-container">
  {#if holdState}
    <div class="circle-container absolute" transition:scale>
      <Circle on:holdEnd={holdEnd} />
    </div>
  {:else}
    <div class="rhombus-container absolute" transition:scale>
      <Rhombus
        {time}
        {displayBreath}
        {scaled}
        {start}
        {countDown}
        {breathCount}
        on:play={play} />
    </div>
  {/if}
</div>

{#if displayBreath}
  <div class="breathe-state absolute" transition:scale>{breatheState}</div>
{/if}
<audio
  src="./meditation.mp3"
  id="audio"
  bind:this={audio}
  loop
  preload="auto" />
<audio src="./tone.mp3" id="tone" bind:this={tone} loop preload="auto" />
