<script>

</script>

<style>
  .circular {
    --size: 30rem;
    --holdTime: 15s;
    width: var(--size);
    height: var(--size);
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
  .circular .inner {
    display: none;
    position: absolute;
    z-index: 6;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);

    height: 80%;
    width: 80%;
    border-radius: 100%;
    background-color: var(--color-1);
    box-shadow: 0 1px 0 rgba(0, 0, 0, 0.2);
  }
  .circular .bar {
    position: absolute;
    width: 100%;
    height: 100%;
    background-color: var(--color-1);
    border-radius: 100%;
    clip: rect(0px, var(--size), var(--size), calc(var(--size) / 2));
  }
  .circular .bar .progress {
    position: absolute;
    width: 100%;
    height: 100%;
    border-radius: 100%;
    clip: rect(0px, calc(var(--size) / 2), var(--size), 0px);
    background: var(--color-2);
  }
  .circle .left .progress {
    z-index: 1;
    animation: left var(--holdTime) linear both;
  }
  @keyframes left {
    100% {
      transform: rotate(180deg);
    }
  }
  .circle .right {
    transform: rotate(180deg);

    z-index: 3;
  }
  .circle .right .progress {
    animation: right var(--holdTime) linear both;
    animation-delay: var(--holdTime);
  }
  @keyframes right {
    100% {
      transform: rotate(180deg);
    }
  }
</style>

<div class="circular" data-pallet="1">
  <div class="inner" />
  <div class="circle">
    <div class="bar left">
      <div class="progress" />
    </div>
    <div class="bar right">
      <div class="progress" />
    </div>
  </div>
</div>
