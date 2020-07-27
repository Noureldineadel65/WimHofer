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
  onMount(() => {
    [...rhombuses.children].forEach((d, i) => {
      d.classList.add("inner-scaled");
      d.style.transition = `transform ${time / 1500}s 0.${i * 2}s`;
    });
  });
  function startBreathing() {
    if (breathingInterval) return;
    displayBreath = true;
    [...rhombuses.children].forEach((d, i) => {
      d.style.transform = `translate(-50%, -50%) scale(1.2)`;
    });
    breathingInterval = setInterval(() => {
      if (!scaled) {
        breathCount++;
        [...rhombuses.children].forEach((d, i) => {
          d.style.transform = `translate(-50%, -50%) scale(0.8)`;
        });
      } else {
        [...rhombuses.children].forEach((d, i) => {
          d.style.transform = `translate(-50%, -50%) scale(1.2)`;
        });
      }
      scaled = !scaled;
    }, time);
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
    margin-top: 8rem;
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
    transform: scale(0.6) rotate(0deg);
  }
  .inner-scaled {
    transform: translate(-50%, -50%) scale(0.5);
  }
  .in {
    color: #fff;
    font-size: 6rem;
    position: absolute;
    letter-spacing: 10px;
    /* margin-left: 10px; */
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%) rotate(-45deg);
    text-shadow: -3px -3px 0 #222, 3px -3px 0 #222, -3px 3px 0 #222,
      3px 3px 0 #222, 4px 4px 0 #fff, 5px 5px 0 #fff, 6px 6px 0 #fff,
      7px 7px 0 #fff;
  }
  .out {
    text-align: center;
    font-size: 8rem;
    position: absolute;
    display: block;
    left: 50%;
    transform: translateX(-50%);
    margin-top: 4rem;
    text-shadow: -3px -3px 0 #222, 3px -3px 0 #222, -3px 3px 0 #222,
      3px 3px 0 #222, 4px 4px 0 #fff, 5px 5px 0 #fff, 6px 6px 0 #fff,
      7px 7px 0 #fff;
  }
</style>

{#if scaled}
  <div class="out" transition:scale>OUT</div>
{/if}

<div
  class="rhombus relative"
  class:scaled
  data-pallet="1"
  on:click={startBreathing}
  bind:this={rhombuses}
  style="transition: transform {time / 1500}s">
  <div class="inner-rhombus absolute inner-rhombus-1" />
  <div class="inner-rhombus absolute inner-rhombus-2" />
  <div class="inner-rhombus absolute inner-rhombus-3" />
  <div class="inner-rhombus absolute inner-rhombus-4">
    {#if !scaled && displayBreath}
      <div class="in" transition:scale>IN</div>
    {/if}
  </div>

</div>
