<script lang="ts">
  let count = $state(0);
</script>

<h1>Counter</h1>
<div class="flex-gap">
  <input type="text" bind:value={count} disabled>
  <button onclick="{() => {count += 1}}">Count</button>
</div>


<style>
  button {
    box-shadow: none;
  }
</style>
