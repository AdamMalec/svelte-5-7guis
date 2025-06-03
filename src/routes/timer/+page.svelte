<script lang="ts">
  let elapsed = $state(0);
  let duration = $state(10);
  let interval: number;

  function start() {
    interval = setInterval(() => {
      elapsed += 0.1;
      if (elapsed > duration) {
        elapsed = duration;
        clearInterval(interval);
      }
    }, 100);
  }

  function reset() {
    elapsed = 0;
    start();
  }

  $effect(() => {
    if (!duration) return;
    start();
    return () => clearInterval(interval);
  });
</script>

<h1>Timer</h1>
<div class="timer">
  <div class="field">
    <label for="">Elapsed Time:</label>
    <progress value={elapsed} max={duration}></progress>
  </div>

  <span>{elapsed.toFixed(1)}'s</span>

  <div class="field">
    <label for="">Duration:</label>
    <span>0</span>
    <input type="range" bind:value={duration} min="0" max="20" />
    <span>20</span>
  </div>

  <button onclick={reset}>Reset</button>
</div>

<style>
  .timer {
    display: grid;
    gap: var(--size-3);
  }

  .field {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: var(--size-2);
  }

  input[type="range"] {
    flex-grow: 1;
  }
</style>
