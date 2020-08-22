<script>
  import Progress from "./Progress.svelte";
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();
  let seconds = 30;
  const holdInterval = setInterval(() => {
    seconds--;
    if (seconds == 0) {
      clearInterval(holdInterval);
      dispatch("holdEnd");
    }
  }, 1000);
</script>

<style>
  .circle {
    --size: 35rem;
    background-color: var(--color-1);
    width: var(--size);
    height: var(--size);
    border-radius: 50%;
    margin: 0 auto;
    overflow: hidden;
  }
  .timer {
    color: var(--color-4);
    font-size: 5rem;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    z-index: 30;
    font-family: "Rowdies", cursive;
    font-variant-numeric: tabular-nums;
  }
</style>

<div class="circle relative">
  <Progress />
  <div class="timer absolute">
    00:{seconds < 10 ? `0${seconds}` : `${seconds}`}
  </div>
</div>
