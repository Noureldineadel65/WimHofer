<script>
  import Tailwindcss from "./Tailwindcss.svelte";
  import { onMount } from "svelte";
  import Header from "./components/Header.svelte";
  import Circle from "./components/Circle.svelte";
  import CreatedBy from "./components/CreatedBy.svelte";
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
  style="background-image: url(./images/ice.jpg)"
  class="flex flex-col justify-between align-center overflow-hidden">
  <Header />
  <Circle />
  <CreatedBy />
</main>
