<script>
  import Tailwindcss from "./Tailwindcss.svelte";
  import { onMount } from "svelte";
  import Header from "./components/Header.svelte";
  import Circle from "./components/Circle.svelte";

  let synth;
  let voices = [];
  let selectedVoice;

  onMount(() => {
    synth = speechSynthesis;
    const getVoices = () => {
      voices = synth.getVoices();
      selectedVoice = voices[0];
    };
    if (synth.onvoiceschanged !== undefined) {
      synth.onvoiceschanged = getVoices;
    }
  });
  $: selectedVoice = voices[0];
</script>

<style>

</style>

<Tailwindcss />
<main>
  <Header />
  <Circle {selectedVoice} />
</main>
