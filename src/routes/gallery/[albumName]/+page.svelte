<script>
  import { page } from "$app/stores";
  import { db } from "../../firebase";

  import { ref, set, get, child, remove } from "firebase/database";

  let galleryName = $page.params.albumName;

  console.log(galleryName);

  let albumData = { albumDate: "", imageLinks: [], title: "" };

  get(ref(db, "gallery/" + galleryName)).then((snapshot) => {
    if (snapshot.exists()) {
      albumData = snapshot.val();
      console.log(albumData);
    } else {
      console.log("damn");
    }
  });
</script>

<main class="prose w-screen">
  <h1>{albumData.title}</h1>
  <p>{albumData.albumDate}</p>

  <hr />
  <div class="overflow-auto w-[95vw] mt-3">
    <div class="masonry">
      {#each albumData.imageLinks as item (item)}
        <div class="masonry-item bg-white shadow rounded-lg relative">
          <img src={item} alt="" class="mt-3" />
        </div>
      {/each}
    </div>
  </div>
</main>

<style>
  .masonry {
    column-count: 6;
    column-gap: 1rem;
  }

  .masonry-item {
    break-inside: avoid;
    margin-bottom: 1rem;
  }
</style>
