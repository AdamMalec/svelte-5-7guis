<script lang="ts">
  type Circle = {
    id: string;
    cx: number;
    cy: number;
    r: number;
  };

  type Status = "draw" | "edit";

  let status = $state<Status>("draw");
  let circles = $state<Circle[]>([]);
  let selectedCircle = $state<Circle>()!;

  let snapshots: Circle[][] = [];
  let historyCounter = $state(-1);

  function drawCircle(e: MouseEvent) {
    if(status === 'edit') {
      status = 'draw';
      snapshot();
      return;
    }

    let svgEl = e.target as SVGElement;
    let { left, top } = svgEl.getBoundingClientRect();

    const circle = {
      id: window.crypto.randomUUID(),
			cx: +(e.clientX - left).toFixed(),
			cy: +(e.clientY - top).toFixed(),
			r: 40,
    };

    circles.push(circle);
    selectedCircle = circle;
    snapshot();
  }

  function undo() { circles = snapshots[--historyCounter] }
  function redo() { circles = snapshots[++historyCounter] }
  function snapshot() {
    historyCounter++;
    snapshots.push($state.snapshot(circles));
  }
</script>

<h1>Circle Drawer</h1>
<div class="container">
  <!-- svelte-ignore a11y_click_events_have_key_events -->
  <!-- svelte-ignore a11y_no_static_element_interactions -->
  <svg class="canvas" onclick={drawCircle} viewBox="0 0 640 420">
    {#each circles as circle}
      <circle
        {...circle}
        stroke="currentColor"
        style="color: var(--surface-4)"
        fill={selectedCircle.id === circle.id ? "coral" : "transparent"}
        stroke-width="3"
        onclick={(e) => {
          e.stopPropagation();
          selectedCircle = circle;
        }}
        oncontextmenu={(e) => {
          // if (status === 'edit') {snapshot()};
          e.preventDefault();
          status = 'edit';
          selectedCircle = circle;
        }}
      ></circle>
    {/each}
  </svg>

  <div class="controls">
    <button onclick={undo} disabled={historyCounter === -1}>Undo</button>
    <button onclick={redo} disabled={historyCounter === snapshots.length - 1}>Redo</button>
  </div>
</div>

{#if status === 'edit'}
  <div class="modal">
    <p>Adjust diameter of a circle at ({selectedCircle.cx}, {selectedCircle.cy}).</p>
    <input type="range" bind:value={selectedCircle.r} style="width: 90%; margin-top: 0.6rem">
  </div>
{/if}

<style>
  .canvas {
    width: 640px;
    height: 420px;
    border: var(--border-size-3) solid var(--surface-4);
    border-radius: var(--radius-2);
  }

  .controls {
    display: flex;
    justify-content: center;
    gap: 0.6rem;
    margin-top: 0.8rem;
  }

  .modal {
    position: absolute;
    top: 60%;
    left: 50%;
    transform: translate(-50%, -50%);

    width: 360px;
    padding: 1rem;
    text-align: center;
    background-color: var(--surface-3);
    border-radius: var(--radius-2);
  }
</style>
