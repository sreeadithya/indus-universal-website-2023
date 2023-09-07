<script>
  import { app, db } from "../firebase";
  import { onMount } from "svelte";
  import { ref, set, get, child, remove } from "/firebase/database";

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
  <div class="py-5 bg-green-300">
    <h2 class="text-lg font-bold">Pinned Announcements</h2>
    <table class="table-auto min-w-full text-left text-sm font-light mt-3">
      <thead />
      <tbody>
        {#each pinnedAnnouncementsTitles as i}
          <tr class="border-b-1">
            <a
              href={"announcement-board/" +
                pinnedAnnouncements[i].title.replaceAll(" ", "_")}
            >
              <td class="whitespace-nowrap px-6 py-4"
                >{pinnedAnnouncements[i].date}</td
              >
              <td class="whitespace-nowrap px-6 py-4"
                >{pinnedAnnouncements[i].title}</td
              >
              <td class="whitespace-nowrap px-6 py-4">
                <div class="flex flex-row gap-4" />
              </td>
            </a>
          </tr>
        {/each}
      </tbody>
    </table>
  </div>

  <div class="py-5 background-neutral-100">
    <h2 class="text-lg font-bold mt-10">Announcements</h2>
    <table class="table-auto min-w-full text-left text-sm font-light mt-3">
      <thead />
      <tbody>
        {#each normalAnnouncementsTitles as i}
          <tr class="border-b-1">
            <a
              href={"announcement-board/" +
                normalAnnouncements[i].title.replaceAll(" ", "_")}
            >
              <td class="whitespace-nowrap px-6 py-4"
                >{normalAnnouncements[i].date}</td
              >
              <td class="whitespace-nowrap px-6 py-4"
                >{normalAnnouncements[i].title}</td
              >
              <td class="whitespace-nowrap px-6 py-4">
                <div class="flex flex-row gap-4" />
              </td>
            </a>
          </tr>
        {/each}
      </tbody>
    </table>
  </div>
</main>
