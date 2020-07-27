<script>
  import Tailwindcss from "./Tailwindcss.svelte";
  import { onMount } from "svelte";
  import Header from "./components/Header.svelte";
  import Circle from "./components/Circle.svelte";
</script>

<style>
  main {
    background-size: cover;
    background-position: center;
    height: 100vh;
  }
</style>

<Tailwindcss />

<main style="background-image: url(./images/ice.jpg)">
  <Header />
  <Circle />
</main>
