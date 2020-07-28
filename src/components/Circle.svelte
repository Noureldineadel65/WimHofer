<script>
  import { onMount, createEventDispatcher, onDestroy } from "svelte";
  import { scale } from "svelte/transition";
  const dispatch = createEventDispatcher();
  let scaled = false;
  let displayBreath = false;
  let breathCount = 0;
  let round = 1;
  let breathingInterval;
  let rhombuses;
  const time = 3500;
  let audio;
  let start = false;
  let tone;
  const countDownTime = 3;
  let countDown = 0;
  const messages = [
    "BRING AWARENESS TO YOUR BREATH",
    "BREATHE IN",
    "BREATHE OUT"
  ];
  let breatheState = "";
  onMount(() => {
    [...rhombuses.children].forEach((d, i) => {
      d.classList.add("inner-scaled");
      d.style.transition = `transform ${time / 1000}s 0.${i * 2}s`;
    });
  });
  function startBreathing() {
    if (breathingInterval) return;
    audio.play();
    displayBreath = true;
    [...rhombuses.children].forEach((d, i) => {
      d.style.transform = `translate(-50%, -50%) scale(1.3)`;
    });
    breathingInterval = setInterval(() => {
      breathCount++;
      if (!scaled) {
        [...rhombuses.children].forEach((d, i) => {
          d.style.transform = `translate(-50%, -50%) scale(0.8)`;
          breatheState = "BREATHE OUT";
        });
      } else {
        [...rhombuses.children].forEach((d, i) => {
          d.style.transform = `translate(-50%, -50%) scale(1.3)`;
          breatheState = "BREATHE IN";
        });
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
  $: console.log(breathCount);
</script>

<style>
  .rhombus {
    background-color: var(--color-1);
    width: 45rem;
    height: 45rem;
    clip-path: polygon(50% 0%, 100% 50%, 50% 100%, 0% 50%);
    margin: 0 auto;
    /* margin-top: 5rem; */
    transform: scale(1) rotate(45deg);
    overflow: hidden;
  }
  .inner-rhombus {
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    clip-path: polygon(50% 0%, 100% 50%, 50% 100%, 0% 50%);
  }
  .inner-rhombus-1 {
    background-color: var(--color-2);

    width: 40rem;
    height: 40rem;
  }
  .inner-rhombus-2 {
    background-color: var(--color-3);

    width: 35rem;
    height: 35rem;
  }
  .inner-rhombus-3 {
    background-color: var(--color-4);

    width: 30rem;
    height: 30rem;
  }
  .inner-rhombus-4 {
    background-color: var(--color-5);
    width: 25rem;
    height: 25rem;
  }
  .scaled {
    transform: scale(0.7) rotate(0deg);
  }
  .breathe-state {
    text-align: center;
    font-size: 2rem;
    font-weight: bolder;
    display: block;

    margin-top: 0rem;
  }
  .play-icon {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%) rotate(-45deg);
    width: 4rem;
  }
  .countDown {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%) rotate(-45deg);
    font-size: 6rem;
  }
</style>

<div
  class="rhombus relative"
  class:scaled
  data-pallet="1"
  on:click={play}
  bind:this={rhombuses}
  style="transition: transform {time / 1500}s">
  <div class="inner-rhombus absolute inner-rhombus-1" />
  <div class="inner-rhombus absolute inner-rhombus-2" />
  <div class="inner-rhombus absolute inner-rhombus-3" />
  <div class="inner-rhombus absolute inner-rhombus-4">
    {#if !displayBreath && !scaled && !start}
      <img
        src="./images/play.svg"
        class="play-icon cursor-pointer"
        transition:scale />
    {:else if start && !displayBreath}
      <div class="countDown font-bolder">{countDown + 1}</div>
    {/if}
  </div>

</div>
{#if displayBreath}
  <div class="breathe-state">
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
