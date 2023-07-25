<script>
  const url = "https://arini.sinathan.com/api/data";

  const fetchData = (async () => {
    const res = await fetch(url);
    const data = await res.json();

    return data;
  })();
</script>

{#await fetchData}
  <p>Loading...</p>
{:then result}
  {#if result.data.length > 0}
    <table class="table">
      <thead>
        <tr>
          <th>No</th>
          <th>Kode</th>
          <th>Nama</th>
          <th>Kelas</th>
        </tr>
      </thead>
      <tbody>
        {#each result.data as data, index}
          <tr>
            <td>{index + 1}</td>
            <td>{data.code}</td>
            <td>{data.name}</td>
            <td>{data.class}</td>
          </tr>
        {/each}
      </tbody>
    </table>
  {:else}
    No Data
  {/if}
{/await}
