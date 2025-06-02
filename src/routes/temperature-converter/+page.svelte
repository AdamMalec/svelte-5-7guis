<script lang="ts">
  class Temperature {
    #c = $state(0);
    #f = $state(0);

    get c() {
      return this.#c;
    }
    set c(value) {
      this.#c = value;
      this.#f = Math.round(value * (9 / 5) + 32);
    }

    get f() {
      return this.#f;
    }
    set f(value) {
      this.#f = value;
      this.#c = Math.round((value - 32) * (5 / 9));
    }
  }

  const temperature = new Temperature();
</script>

<h1>Temperature<br />Converter</h1>
<div class="converter">
  <div class="field">
    <label for="">Celsius:</label>
    <input type="text" bind:value={temperature.c} />
  </div>

  <span>=</span>

  <div class="field">
    <label for="">Fahrenheit:</label>
    <input type="text" bind:value={temperature.f} />
  </div>
</div>

<style>
  .converter {
    display: flex;
    justify-content: center;
    align-items: flex-end;
    gap: var(--size-2);
  }

  .field {
    display: flex;
    flex-direction: column;
    gap: var(--size-1);

    width: var(--size-10);
  }

  span {
    padding-bottom: var(--size-2);
  }
</style>
