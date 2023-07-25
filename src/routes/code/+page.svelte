<script>
  import { onMount } from "svelte";

  const url = "https://arini.sinathan.com/api/code";

  let validationCode = null;
  let validationCodeId = null;

  onMount(async () => {
    const res = await fetch(url);
    const json = await res.json();

    if (json.data != null) {
      validationCodeId = json.data.id;
      validationCode = json.data.code;
    }
  });

  const generateCode = async () => {
    const res = await fetch(url, {
      method: "POST",
    });

    const json = await res.json();
    validationCodeId = json.data.id;
    validationCode = json.data.code;
  };

  const deleteCode = async () => {
    const res = await fetch(url + "/" + validationCodeId, {
      method: "DELETE",
    });
    validationCode = null;
  };
</script>

{#if validationCode}
  <h6>Code:</h6>
  <h1>{validationCode}</h1>
  <button class="btn btn-danger" on:click={deleteCode}>Delete Code</button>
{:else}
  <button class="btn btn-primary" on:click={generateCode}>Generate Code</button>
{/if}
