<script>
  import { onMount } from "svelte";
  import hoverEffect from "hover-effect";
  let backgrounds;
  let hover;
  export let next = false;
  export let before = false;
  onMount(() => {
    hover = new hoverEffect({
      parent: backgrounds,
      image1: "./images/beach.jpg",
      image2: "./images/ice.jpg",
      displacementImage: "./images/heightMap.png",
      hover: false
    });
  });
  $: next ? hover.next() : void 0;
  $: before ? hover.previous() : void 0;
</script>

<style>
  .backgrounds::after {
    content: "";
    background: rgba(34, 34, 34, 0.4);
    width: 100vw;
    height: 100vh;
    position: absolute;
  }
</style>

<div class="backgrounds absolute w-full h-full" bind:this={backgrounds} />
