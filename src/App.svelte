<script>
  import Tailwindcss from "./Tailwindcss.svelte";
  import { onMount } from "svelte";
  import Header from "./components/Header.svelte";
  import Meditation from "./components/Meditation.svelte";
  import CreatedBy from "./components/CreatedBy.svelte";
</script>

<style>
  main {
    background-size: cover;
    background-position: center;
    height: 100vh;
  }

  .backgrounds::after {
    content: "";
    background: rgba(34, 34, 34, 0.534);
    width: 100vw;
    height: 100vh;
    position: absolute;
  }
  /* .backgrounds img {
    object-fit: cover;
  } */
</style>

<Tailwindcss />

<main
  class="flex flex-col justify-between align-center overflow-hidden relative">
  <div class="backgrounds absolute w-full h-full">
    <img src="./images/beach.jpg" class="absolute object-cover w-full h-full" />
  </div>
  <Header />
  <div class="meditation-container relative">
    <Meditation />
  </div>
  <CreatedBy />
</main>
