<script lang="ts">
  type User = {
    name: string;
    surname: string;
  };

  let users = $state([
    { name: "Tom", surname: "Thibodeau" },
    { name: "Michael", surname: "Malone" },
    { name: "Taylor", surname: "Jenkins" },
  ]);

  let prefix = $state("");
  let selected = $state<User>();
  let currentUser = $state({ name: "", surname: "" });
  let filteredUsers = $derived(
    prefix ?
    users.filter((user) => user.surname.toLocaleLowerCase().startsWith(prefix)) :
    users
  );

  function createUser() {
    users.push(currentUser);
    clearFields();
  }
  function updateUser() {
    let index = users.indexOf(selected!);
    users[index] = { name: currentUser.name, surname: currentUser.surname };
  }
  function deleteUser() {
    users = users.filter(
      (user) =>
        user.name !== currentUser.name || user.surname !== currentUser.surname,
    );
    clearFields();
  }

  function clearFields() {
    currentUser = { name: "", surname: "" };
  }

  $effect(() => {
    currentUser = {
      name: selected?.name || "",
      surname: selected?.surname || "",
    };
  });
</script>

<h1>CRUD</h1>
<div class="wrapper">
  <div class="field search">
    <label for="search">Filter&nbsp;prefix:</label>
    <input type="text" name="search" id="search" bind:value={prefix} />
  </div>

  <select
    class="users"
    name="users"
    style="height: 144px"
    bind:value={selected}
    size="5"
  >
    {#each filteredUsers as user}
      <option value={user}>{user.name} {user.surname}</option>
    {/each}
  </select>

  <div class="details">
    <div class="field">
      <label for="name">Name:</label>
      <input type="text" name="name" bind:value={currentUser.name} />
    </div>
    <div class="field">
      <label for="surname">Surname:</label>
      <input type="text" name="surname" bind:value={currentUser.surname} />
    </div>
  </div>

  <div class="controls">
    <button onclick={createUser}>Create</button>
    <button onclick={updateUser}>Update</button>
    <button onclick={deleteUser}>Delete</button>
  </div>
</div>

<style>
  .wrapper {
    display: grid;
    grid-template-areas:
      "search ."
      "users details"
      "controls .";
    grid-template-columns: 280px 1fr;
    row-gap: var(--size-3);
    column-gap: var(--size-5);
    width: 520px;

    @media screen and (width <= 576px) {
      grid-template-areas:
        "search"
        "users"
        "details"
        "controls";

      grid-template-columns: 1fr;
      width: 300px;
    }
  }

  .field {
    display: flex;
    justify-content: space-between;
    gap: var(--size-3);

    input {
      width: calc(64% - var(--size-3));
    }
  }

  .search {
    grid-area: search;
  }

  .users {
    grid-area: users;
  }

  .details {
    grid-area: details;

    .field {
      align-items: baseline;

      &:last-child {
        margin-top: var(--size-2);
      }
    }
  }

  .controls {
    grid-area: controls;
    display: flex;
    justify-content: space-between;
  }
</style>
