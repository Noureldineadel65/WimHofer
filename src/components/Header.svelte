<style>
  h1 {
    font-size: 6rem;
    @apply text-center;
  }
  .setting-icon {
    right: 0;
    top: 50%;
    transform: translateY(-50%);
    margin: 0 4rem;
    cursor: pointer;
  }
  .setting-icon:hover img {
    transform: rotate(360deg);
  }
</style>

<header class="relative">
  <h1 class="text-white">WimHofer</h1>
  <div class="setting-icon absolute w-20">
    <img
      src="./images/setting.svg"
      alt="settings-icon"
      class="w-full transition-all duration-500" />
  </div>
</header>
