<script>
  import BtnForm from "./lib/btnForm.svelte";
  import SColorblind from "./lib/sColorblind.svelte";
  import SButtons from "./lib/sButtons.svelte";

  let showButtons = false;
  let showColorblind = false;
  let data;

  const generate = (d) => {
    data = d.detail;
    console.log(data)
    showButtons = true;
  }
</script>

<main>
  {#if showButtons}
    {#if showColorblind}
      <SColorblind details={data} on:goBack={() => showColorblind = false}/>
    {:else}
      <SButtons details={data} on:goBack={() => showButtons = false} 
        on:colorBlind={() => showColorblind = true}/>
    {/if}
  {:else}
    <BtnForm on:generate={generate} details={data}/>
  {/if}
</main>

<style>
</style>
