<script>
  import {
    ref,
    getDownloadURL,
    uploadBytesResumable,
    deleteObject,
    listAll,
  } from "firebase/storage";
  import { storage, db } from "../firebase";
  import {
    ref as dbref,
    set,
    push,
    update,
    get,
    remove,
  } from "firebase/database";

  let albums;
  let albumTitles = [];

  get(dbref(db, "gallery/")).then((snapshot) => {
    if (snapshot.exists()) {
      albums = snapshot.val();

      albumTitles = Object.keys(albums);
    } else {
      albums = {};
      albumTitles = [];
    }
  });
</script>

<main>
  <div class="overflow-auto w-[95vw] mt-3">
    <div class="masonry">
      {#each albumTitles as i}
        <div class="masonry-item bg-white relative h-max">
          <a href={"gallery/" + i.replaceAll(" ", "_")} class="relative">
            <img
              src={albums[i].imageLinks[
                Math.floor(Math.random() * (albums[i].imageLinks.length - 0))
              ]}
              alt=""
            />

            <div class="absolute left-0 bottom-0 bg-slate-300">
              <p class="whitespace-nowrap px-6">
                {i.replaceAll("_", " ")}
              </p>
              <p class="whitespace-nowrap px-6 text-xs">
                {albums[i].albumDate}
              </p>
            </div>
          </a>
        </div>
      {/each}
    </div>
  </div>
</main>

<style>
  .masonry {
    column-count: 3;
    column-gap: 1rem;
  }

  .masonry-item {
    break-inside: avoid;
    margin-bottom: 1rem;
  }
</style>
