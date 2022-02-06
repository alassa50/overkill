<script lang="ts">
  import "@picocss/pico"
  import Compressor from "compressorjs";
  let files;
  let formData = new FormData();
  const serverURL = "http://localhost:5050/upload";
  
  const handlepic = (e, name) => {
    const extension = e.target.files[0].type.split("/")[1];

    new Compressor(e.target.files[0], {
      quality: 0.4,
      success(resultCompressed) {
        formData.append("photo", resultCompressed, `${name}_${Date.now()}_.${extension}`);
      },
      error(err) {
        console.log(err.message);
      },
    });
  };

  const uploaPics = async (url) => {
    try {
      const response = await fetch(url, {
        method: "POST",
        body: formData,
      });
      console.log(JSON.stringify(response, null, 2));
      if (response.ok) {
        console.log("Uploaded successfully!");
        files = null;
        formData = new FormData();
        document.getElementById('picForm').reset();
      } else {
        console.log("Failed to upload!");
      }
    } catch (e) {
      console.log(e);
    }
  };
</script>


<!-- Header -->
<header>
  <hgroup>
    <h1>Overkill</h1>
    <h2>
      Pico design avec Svelte pour le projet Overkill
    </h2>
  </hgroup>

</header>
<!-- ./ Header -->

<!-- Main -->
<main>
  <h1>Picture Upload</h1>
  <form enctype="multipart/form-data" id="picForm">
    <div class="grid">
    <label><span>Front Picture</span>
      <input
        type="file"
        accept="image/*"
        required
        bind:files
        name="photo"
        on:change={(e) => handlepic(e, 'front')} />
    </label>
    <label><span>Back Picture</span>
      <input
        type="file"
        accept="image/*"
        required
        bind:files
        name="photo"
        on:change={(e) => handlepic(e, 'back')} />
    </label>
    <label><span>Side Picture</span>
      <input
        type="file"
        accept="image/*"
        required
        bind:files
        name="photo"
        on:change={(e) => handlepic(e, 'side')} />
    </label>
    <button on:click|preventDefault={() => uploaPics(serverURL)}>Upload</button>
  </div>
  </form>

</main>
<!-- ./ Main -->

<!-- Footer -->
<footer class="container">
  
</footer>
<!-- ./ Footer -->
