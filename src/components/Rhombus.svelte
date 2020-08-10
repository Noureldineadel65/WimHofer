<script>
  import { onMount, createEventDispatcher, onDestroy } from "svelte";
  import { scale } from "svelte/transition";
  const dispatch = createEventDispatcher();
  export let time = 0;
  export let displayBreath = false;
  export let scaled = false;
  export let start = false;
  export let countDown = 0;
  export let breathCount = 0;
  let rhombuses;
  let rhombusChildren;
  onMount(() => {
    [...rhombuses.children].forEach((d, i) => {
      d.classList.add("inner-scaled");
      d.style.transition = `transform ${time / 1000}s 0.${i * 2}s`;
    });
    console.log([...rhombuses.children]);
  });

  function scaleRhombus() {
    console.log(rhombuses);
    setTimeout(() => {
      [...rhombuses.children].forEach((d, i) => {
        d.style.transform = `translate(-50%, -50%) scale(1.3)`;
      });
    }, 0);
  }

  function unScaleRhombus() {
    console.log(rhombuses);
    setTimeout(() => {
      [...rhombuses.children].forEach((d, i) => {
        d.style.transform = `translate(-50%, -50%) scale(0.8)`;
      });
    }, 0);
  }
  $: if (scaled) {
    unScaleRhombus();
  } else if (!scaled && displayBreath) {
    scaleRhombus();
  }
</script>

<style>
  .rhombus {
    --size: 45rem;
    background-color: var(--color-1);
    width: var(--size);
    height: var(--size);
    clip-path: polygon(50% 0%, 100% 50%, 50% 100%, 0% 50%);
    margin: 0 auto;
    transform: scale(1) rotate(45deg);

    overflow: hidden;
  }
  .inner-rhombus {
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    clip-path: polygon(50% 0%, 100% 50%, 50% 100%, 0% 50%);
    border-radius: 50%;
  }
  .inner-rhombus-1 {
    background-color: var(--color-2);

    width: calc(var(--size) - 5rem);
    height: calc(var(--size) - 5rem);
  }
  .inner-rhombus-2 {
    background-color: var(--color-3);

    width: calc(var(--size) - 10rem);
    height: calc(var(--size) - 10rem);
  }
  .inner-rhombus-3 {
    background-color: var(--color-4);

    width: calc(var(--size) - 15rem);
    height: calc(var(--size) - 15rem);
  }
  .inner-rhombus-4 {
    background-color: var(--color-5);
    width: calc(var(--size) - 20rem);
    height: calc(var(--size) - 20rem);
  }
  .scaled {
    transform: scale(0.7) rotate(0deg);
    border-radius: 50%;
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
  .breatheCount {
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
  on:click={() => dispatch('play')}
  bind:this={rhombuses}
  style="transition: all {time / 1500}s">
  <div class="inner-rhombus absolute inner-rhombus-1" />
  <div class="inner-rhombus absolute inner-rhombus-2" />
  <div class="inner-rhombus absolute inner-rhombus-3" />
  <div class="inner-rhombus absolute inner-rhombus-4">
    {#if !displayBreath && !scaled && !start}
      <img
        src="./images/play.svg"
        class="play-icon cursor-pointer"
        transition:scale
        alt="..." />
    {:else if start && !displayBreath}
      <div class="countDown font-bolder" transition:scale>{countDown + 1}</div>
    {:else if displayBreath && !scaled}
      <div class="breatheCount" transition:scale>{breathCount}</div>
    {/if}
  </div>

</div>
