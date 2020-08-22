<script>
  import { onMount, createEventDispatcher, onDestroy } from "svelte";
  import Rhombus from "./Rhombus.svelte";
  import Circle from "./Circle.svelte";
  import { scale, fly } from "svelte/transition";
  const dispatch = createEventDispatcher();
  let scaled = false;
  let displayBreath = false;
  let start = false;
  let holdState = false;
  let fullyState = false;
  let roundEnd = false;
  let breathCount = 0;
  let countDown = 0;
  let round = 1;
  let middleFull = false;
  const countDownTime = 3;
  const time = 3000;
  let fullyTimer = "";
  let breathingInterval;
  let audio;
  let tone;
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
        if (breathCount === 2) {
          clearInterval(breathingInterval);
          breathingInterval = null;
          breathCount = 0;
          middleFull = true;
          finishBreathing();
        }
      }
      scaled = !scaled;
    }, time);
  }
  function finishBreathing() {
    // displayBreath = false;
    // start = false;
    breathCount = "";
    breatheState = "FULLY IN!";

    // breatheState = "HOLD YOUR BREATH";

    // holdState = true;
    // stopAudio(audio);
    setTimeout(() => fullyIn(), time);
  }
  function reset() {
    scaled = false;
    displayBreath = false;
  }
  function endRound() {
    reset();
    roundEnd = true;
    setTimeout(() => {
      roundEnd = false;

      setTimeout(() => {
        start = true;
        round++;
        startBreathing();
      });
    }, 5000);
  }
  function fullyIn() {
    breatheState = "FULLY IN!";
    displayBreath = true;
    scaled = false;
    let count = 15;
    if (middleFull) {
      count = 5;
      fullyState = false;
    } else {
      fullyState = true;
    }

    fullyTimer = `00:${count < 10 ? `0${count}` : `${count}`}`;
    let full = setInterval(() => {
      count--;
      fullyTimer = `00:${count < 10 ? `0${count}` : `${count}`}`;

      if (count === 0) {
        fullyState = false;
        fullyTimer = "";

        if (middleFull) {
          displayBreath = false;
          start = false;
          breatheState = "HOLD YOUR BREATH";
          holdState = true;
          stopAudio(audio);
        } else {
          endRound();
        }
        clearInterval(full);
      }
    }, 1000);
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
    reset();
    holdState = false;
    middleFull = false;
    setTimeout(() => {
      fullyIn();
    }, 0);
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
  .circle-container,
  .roundEnd {
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
  .roundEnd {
    font-size: 4rem;
    border: 5px solid var(--color-1);
    color: #fff;
    z-index: 4;
    padding: 2rem;
    animation: animateColor 5s;
    animation-fill-mode: forwards;
  }
  .roundEnd span {
    width: 5rem;
    height: 5rem;
    background-color: var(--color-1);
    bottom: -100%;
    left: 50%;
    border-radius: 50%;
    z-index: -1;
    transform: translateX(-50%);
    animation: scaleSpan 5s;
    animation-fill-mode: forwards;
  }
  @keyframes scaleSpan {
    0% {
      transform: translateX(-50%) scale(0);
    }
    100% {
      transform: translateX(-50%) scale(20);
    }
  }
  @keyframes animateColor {
    0% {
      color: #fff;
    }
    100% {
      color: var(--color-4);
    }
  }
</style>

{#if start}
  <div class="round text-center" transition:scale>ROUND {round}</div>
{/if}
<div class="shape-container" data-pallet="1">
  {#if holdState}
    <div class="circle-container absolute" transition:scale>
      <Circle on:holdEnd={holdEnd} />
    </div>
  {:else if roundEnd}
    <div class="roundEnd absolute overflow-hidden" tansition:fly={{ y: -100 }}>
      ROUND {round} ENDED
      <span class="absolute" />
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
        {fullyState}
        {fullyTimer}
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
  preload="auto"
  muted />
<audio src="./tone.mp3" id="tone" bind:this={tone} loop preload="auto" muted />
