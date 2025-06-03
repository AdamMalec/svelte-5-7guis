<script lang="ts">
  type Options = "one-way" | "return";

  let selected = $state<Options>("one-way");
  let startDate = $state(getDate());
  let returnDate = $state(getDate());

  function getDate() {
    const [month, day, year] = new Date()
      .toLocaleDateString("en-US", {
        year: "numeric",
        month: "2-digit",
        day: "2-digit",
      })
      .split("/");

    return `${year}-${month}-${day}`;
  }

  // todo Add  success notification
  function handleSubmit(e: Event) {}

  $inspect({ selected, startDate, returnDate });
</script>

<h1>Flight Booker</h1>
<form onsubmit={handleSubmit}>
  <select name="flight" bind:value={selected}>
    <option value="one-way">one-way flight</option>
    <option value="return">return flight</option>
  </select>

  <input type="date" bind:value={startDate} min={getDate()} required />
  <input
    type="date"
    bind:value={returnDate} min={getDate()}
    disabled={selected !== "return"}
    required
  />

  <button
    type="submit"
    disabled={!startDate || (selected === "return" && returnDate < startDate)}
    >Book</button
  >
</form>

<style>
  form {
    display: grid;
    gap: var(--size-2);

    max-width: 20ch;
    margin: 0 auto;
  }

  input[disabled] {
    background-color: var(--surface-2);
    opacity: 0.5;
  }

  button {
    padding-block: var(--size-relative-2);
  }
</style>
