<script>
  import { onMount } from "svelte";

  const url = "https://arini.sinathan.com/api";

  let nama = "";
  let kelas = "";
  let kode = "";
  let result = null;
  let validationCode = null;

  onMount(async () => {
    const res = await fetch(url + '/code');
    const json = await res.json();

    if (json.data != null) {
      validationCode = json.data.code;
    }
  });

  async function checkIn() {
    result = null;

    let formData = new FormData();
    formData.append("code", kode);
    formData.append("name", nama);
    formData.append("class", kelas);

    if (nama && kelas && kode) {
      if (kode == validationCode) {
        const res = await fetch(url + '/data', {
          method: "POST",
          body: formData,
        });

        const json = await res.json();
        result = json;

        nama = "";
        kelas = "";
        kode = "";
      } else {
        result = 0;
      }
    } else {
      result = 0;
    }
  }
</script>

{#if result !== null}
  {#if result.status == 1}
    <div class="alert alert-success alert-dismissible fade show" role="alert">
      Check-in Berhasil!
      <button
        type="button"
        class="btn-close"
        data-bs-dismiss="alert"
        aria-label="Close"
      />
    </div>
  {:else if result == 0}
    <div class="alert alert-danger alert-dismissible fade show" role="alert">
      Check-in Gagal!
      <button
        type="button"
        class="btn-close"
        data-bs-dismiss="alert"
        aria-label="Close"
      />
    </div>
  {/if}
{/if}
<div class="text-start">
  <div class="form-floating mb-3">
    <input
      type="text"
      class="form-control"
      id="name"
      name="name"
      placeholder="Nama Anda"
      bind:value={nama}
      required
    />
    <label for="name">Nama</label>
  </div>
  <div class="form-floating mb-3">
    <input
      type="text"
      class="form-control"
      id="class"
      name="class"
      placeholder="Kelas Anda"
      bind:value={kelas}
      required
    />
    <label for="class">Kelas</label>
  </div>
  <div class="form-floating mb-3">
    <input
      type="number"
      class="form-control"
      id="code"
      name="code"
      placeholder="Kode Absensi"
      bind:value={kode}
      required
    />
    <label for="code">Kode Absensi</label>
  </div>
  <button class="btn btn-primary w-100" type="button" on:click={checkIn}
    >Check-in</button
  >
</div>
