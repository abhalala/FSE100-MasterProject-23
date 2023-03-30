<script>
  import { writable } from "svelte/store";
  import Pinch from "./Pinch.svelte";

  let selectedActivity = writable("select");
</script>

<!-- <div class="overflow-auto h-screen w-screen grid ">
  {#if $selectedActivity === "select"}
    <h1
      class="text-4xl text-white uppercase font-bold px-3 bg-gray-800 rounded-md"
    >
      Select an activity
    </h1>
    <div
      class="bg-gray-900 text-white grid justify-center h-full w-screen font-mono uppercase font-bold p-3"
    >
      <div
        class="flex space-y-3 flex-col justify-center items-center h-full bg-gray-900 text-white font-mono uppercase"
      >
        <button
          class="bg-blue-500 hover:bg-blue-700 text-white text-4xl font-sans uppercase font-bold p-10 h-full w-full rounded"
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
</div> -->

<div
  class="bg-gray-500 space-y-3 h-screen overflow-auto grid grid-cols-1 p-3 w-screen"
>
  {#if $selectedActivity === "select"}
    <h1
      class="font-bold uppercase text-5xl font-sans text-white grid justify-center place-content-center"
    >
      Select an activity
    </h1>

    <div class="space-y-5 m-5">
      <button
        on:click={() => selectedActivity.set("pinch")}
        class="p-5 text-white border-4 text-5xl border-gray-400 rounded-md bg-black font-sans w-full uppercase font-bold"
      >
        Pinch
      </button>

      <button
        on:click={() => selectedActivity.set("trace")}
        class="p-5 text-white border-4 text-5xl border-gray-400 rounded-md bg-black font-sans w-full uppercase font-bold"
      >
        Tracing
      </button>

      <button
        on:click={() => selectedActivity.set("jar")}
        class="p-5 text-white border-4 text-5xl border-gray-400 rounded-md bg-black font-sans w-full uppercase font-bold"
      >
        Opening a Jar
      </button>
    </div>
  {:else if $selectedActivity === "pinch"}
    <div class="grid justify-center place-content-center space-y-2">
      <button
        on:click={() => selectedActivity.set("select")}
        class="font-sans font-bold text-2xl text-white bg-green-500 p-3 rounded-md"
      >
        Back
      </button>
      <h1 class="font-bold font-sans text-white text-3xl uppercase">Pinch</h1>
    </div>
    <div
      class="border-4 border-gray-400 bg-black rounded-md overflow-hidden m-3"
    >
      <Pinch />
    </div>
  {:else if $selectedActivity === "trace"}
    <div class="grid justify-center place-content-center space-y-2">
      <button
        on:click={() => selectedActivity.set("select")}
        class="font-sans font-bold text-2xl text-white bg-green-500 p-3 rounded-md"
      >
        Back
      </button>
      <h1 class="font-bold font-sans text-white text-3xl uppercase">Tracing</h1>
    </div>
    <div
      class="border-4 border-gray-400 text-white bg-black rounded-md overflow-hidden m-3"
    >
      WIP
    </div>
  {:else if $selectedActivity === "jar"}
    <div class="grid justify-center place-content-center space-y-2">
      <button
        on:click={() => selectedActivity.set("select")}
        class="font-sans font-bold text-2xl text-white bg-green-500 p-3 rounded-md"
      >
        Back
      </button>
      <h1 class="font-bold font-sans text-white text-3xl uppercase">
        Opening a jar
      </h1>
    </div>
    <div
      class="border-4 border-gray-400 text-white bg-black rounded-md overflow-hidden m-3"
    >
      WIP
    </div>
  {/if}
</div>
