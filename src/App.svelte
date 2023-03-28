<script>
  import { writable } from "svelte/store";
  import Pinch from "./Pinch.svelte";

  let selectedActivity = writable("select");
</script>

<div class="overflow-auto">
  {#if $selectedActivity === "select"}
    <div
      class="bg-gray-900 text-white grid justify-center font-mono uppercase font-bold p-3"
    >
      <h1 class="text-4xl px-3 bg-gray-800 rounded-md">Select an activity</h1>
      <div
        class="flex space-y-3 flex-col justify-center items-center h-screen bg-gray-900 text-white font-mono uppercase"
      >
        <button
          class="bg-blue-500 hover:bg-blue-700 text-white font-bold p-10 h-full w-full rounded"
          on:click={() => selectedActivity.set("pinch")}
        >
          Pinch
        </button>
      </div>
    </div>
  {/if}

  {#if $selectedActivity === "pinch"}
    <div class="h-screen w-screen">
      <Pinch />
    </div>
  {/if}
</div>
