<script>
  import Tailwindcss from "./Tailwindcss.svelte";
  import { onMount } from "svelte";
  import Header from "./components/Header.svelte";
  import Meditation from "./components/Meditation.svelte";
  import Background from "./components/Background.svelte";
  import CreatedBy from "./components/CreatedBy.svelte";
  let next = false;
  let before = false;
</script>

<style>
  main {
    background-size: cover;
    background-position: center;
    height: 100vh;
  }
</style>

<Tailwindcss />

<main
  class="flex flex-col justify-between align-center overflow-hidden relative">
  <Background {next} {before} />
  <Header />
  <div class="meditation-container relative">
    <Meditation
      on:holdStart={() => {
        next = true;
        before = false;
      }}
      on:holdEnd={() => {
        next = false;
        before = true;
      }} />
  </div>
  <CreatedBy />
</main>
