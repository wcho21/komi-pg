<script lang="ts">
  import { onMount } from "svelte";
  import config from "$lib/config";

  let sourceForm: HTMLFormElement;
  let sourceElem: HTMLTextAreaElement;
  let rendererElem: HTMLParagraphElement;

  onMount(async () => {
    const komi = await import(/* @vite-ignore */ config.komiUrl); // TODO: handle loading fail
    const init = komi.default;
    await init();

    const execute = komi.execute;
    sourceForm.addEventListener("submit", () => {
      const source = getSource();
      const executed = execute(source); // TODO: handle interpreter fail
      renderExecutedResult(executed);
    });
  });

  function getSource(): string {
    const source = sourceElem.value;
    return source;
  }

  function renderExecutedResult(result: string): void {
    rendererElem.textContent = result;
  }
</script>

<div class="p-1 w-full h-dvh flex flex-col">
  <h1 class="mb-2 flex-0">Playground</h1>
  <div class="flex-1 flex gap-2">
    <div class="flex-1 flex">
      <form id="source-form" class="flex-1 flex flex-col gap-2" bind:this={sourceForm}>
        <textarea class="border-1 border-gray-200 w-full flex-1" bind:this={sourceElem}></textarea>
        <button class="border-1 border-gray-200 w-full flex-0 py-3">Execute</button>
      </form>
    </div>
    <div class="flex-1">
      <p class="border-1 border-gray-200 w-full h-full" bind:this={rendererElem}></p>
    </div>
  </div>
</div>