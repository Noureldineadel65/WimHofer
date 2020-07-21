<script>
  import { onMount, createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();
  let scaled = false;
  export let selectedVoice;
  $: console.log(selectedVoice);
</script>

<style>
  .circle {
    background-color: rgba(255, 255, 255, 0.267);
    width: 25rem;
    height: 25rem;
    border-radius: 50%;
    margin: 15rem auto;
  }
  .inner-circle {
    background-color: rgb(255, 255, 255);
    width: 25rem;
    height: 25rem;
    border-radius: 50%;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%) scale(1);
    transition: transform 2s;
  }
  .scaled {
    transform: translate(-50%, -50%) scale(0.3);
  }
</style>

<div class="circle relative" on:click={() => (scaled = !scaled)}>
  <div class="inner-circle absolute" class:scaled />
</div>
