<script>
  import { app, db } from "../firebase";
  import { onMount } from "svelte";
  import { ref, set, get, child, remove } from "firebase/database";

  let announcements;
  let announcementsTitles = [];
  let pinnedAnnouncements = [];
  let normalAnnouncements = [];
  let pinnedAnnouncementsTitles = [];
  let normalAnnouncementsTitles = [];

  function getData() {
    announcements = {};
    announcementsTitles = {};
    pinnedAnnouncements = [];
    normalAnnouncements = [];
    pinnedAnnouncementsTitles = [];
    normalAnnouncementsTitles = [];
    get(ref(db, "announcements/")).then((snapshot) => {
      if (snapshot.exists()) {
        announcements = snapshot.val();

        announcementsTitles = Object.keys(announcements);

        for (let i = 0; i < announcementsTitles.length; i++) {
          if (announcements[announcementsTitles[i]].pinned == true) {
            pinnedAnnouncements = [
              ...pinnedAnnouncements,
              announcements[announcementsTitles[i]],
            ];
          } else {
            normalAnnouncements = [
              ...normalAnnouncements,
              announcements[announcementsTitles[i]],
            ];
          }
        }
        pinnedAnnouncementsTitles = Object.keys(pinnedAnnouncements);
        normalAnnouncementsTitles = Object.keys(normalAnnouncements);
      } else {
        announcements = {};
        announcementsTitles = {};
        pinnedAnnouncements = [];
        normalAnnouncements = [];
        pinnedAnnouncementsTitles = [];
        normalAnnouncementsTitles = [];
      }
    });
  }

  onMount(() => {
    getData();
  });
</script>

<main>
  <h2 class="text-lg font-bold">Announcements</h2>
  <div class="py-5 border-[#00000080] border-2 border-solid p-8 rounded-md m-4">
    <p class=" font-bold">Pinned</p>
    <div
      class="all-announcements py-5 grid grid-cols-4 gap-6 pr-3 max-[1000px]:grid-cols-1 text-black"
    >
      {#each pinnedAnnouncementsTitles as i}
        <a
          href={"announcement-board/" +
            pinnedAnnouncements[i]["title"].replaceAll(" ", "_")}
        >
          <div class="w-[100%]">
            <img
              src={pinnedAnnouncements[i].thumbnail}
              alt=""
              class="rounded-md w-max h-max"
            />

            <div class="flex justify-between w-[100%] pt-2">
              <p>{pinnedAnnouncements[i].title}</p>

              <p>
                {pinnedAnnouncements[i].date.split("-").reverse().join("-")}
              </p>
            </div>
          </div>
        </a>
      {/each}
    </div>
  </div>

  <div class="py-5 border-[#00000080] border-2 border-solid p-8 rounded-md m-4">
    <div
      class="all-announcements py-5 grid grid-cols-4 gap-6 pr-3 max-[1000px]:grid-cols-1 text-black"
    >
      {#each normalAnnouncementsTitles as i}
        <a
          href={"announcement-board/" +
            normalAnnouncements[i]["title"].replaceAll(" ", "_")}
        >
          <div class="w-[100%]">
            <img
              src={normalAnnouncements[i].thumbnail}
              alt=""
              class="rounded-md w-max h-max"
            />

            <div class="flex justify-between w-[100%] pt-2">
              <p>{normalAnnouncements[i].title}</p>

              <p>
                {normalAnnouncements[i].date.split("-").reverse().join("-")}
              </p>
            </div>
          </div>
        </a>
      {/each}
    </div>
  </div>
</main>
