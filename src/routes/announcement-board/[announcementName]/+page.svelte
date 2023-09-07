<script>
  import { page } from "$app/stores";
  import { db } from "../../firebase";

  import { ref, set, get, child, remove } from "firebase/database";

  let announcementName = $page.params.announcementName;
  let announcementData = { data: [], date: "", pinned: false, title: "" };
  let html = "";
  let blockType;
  let blockTag;
  let blockClosingTag;
  let tableRowClose;

  get(ref(db, "announcements/" + announcementName)).then((snapshot) => {
    announcementData = snapshot.val();

    console.log(announcementData.data);

    for (let key in announcementData.data.blocks) {
      blockType = announcementData.data.blocks[key].type;
      console.log(blockType);

      if (blockType == "paragraph") {
        blockTag = "<p>";
        blockClosingTag = "</p>";
        html = html.concat(
          blockTag +
            announcementData.data.blocks[key].data.text +
            blockClosingTag
        );
        html = html.replace("<a", "<a target='_blank'");
      }

      if (blockType == "attaches") {
        html = html.concat(
          `<button onclick='window.open("${announcementData.data.blocks[key].data.file.url}")'>${announcementData.data.blocks[key].data.title}</button>` +
            "</button>"
        );
      }

      if (blockType == "image") {
        html = html.concat(
          `<img src='${announcementData.data.blocks[key].data.file.url}")'>` +
            "</img>" +
            "<p> caption: " +
            announcementData.data.blocks[key].data.caption +
            "</p>"
        );
      }

      if (blockType == "header") {
        blockTag = `<h${announcementData.data.blocks[key].data.level}>`;
        blockClosingTag = `</h${announcementData.data.blocks[key].data.level}>`;
        html = html.concat(
          blockTag +
            announcementData.data.blocks[key].data.text +
            blockClosingTag
        );
      }

      if (blockType == "List") {
        for (let itemKey in announcementData.data.blocks[key].data.items) {
          blockTag = "<ul>";
          blockClosingTag = "</ul>";
          html = html.concat(
            blockTag +
              announcementData.data.blocks[key].data.items[itemKey] +
              blockClosingTag
          );
        }
      }

      if (blockType == "table") {
        html = html.concat("<table>");
        if (announcementData.data.blocks[key].data.withHeadings == true) {
          html = html.concat("<th><tr>");
        }
        for (let rowKey in announcementData.data.blocks[key].data.content) {
          html = html.concat("<tr>");
          for (let elementKey in announcementData.data.blocks[key].data.content[
            rowKey
          ]) {
            if (
              announcementData.data.blocks[key].data.withHeadings == true &&
              rowKey == 0
            ) {
              html = html.concat(
                "<th>" +
                  announcementData.data.blocks[key].data.content[rowKey][
                    elementKey
                  ] +
                  "</th>"
              );
            } else {
              html = html.concat(
                "<td>" +
                  announcementData.data.blocks[key].data.content[rowKey][
                    elementKey
                  ] +
                  "</td>"
              );
            }
          }
          html = html.concat("</tr>");
        }

        if (announcementData.data.blocks[key].data.withHeadings == true) {
          html = html.concat("</tr></th>");
        }

        html = html.concat("</table>");
      }
    }

    console.log(html);
  });
</script>

<main class="prose">
  <h1>{announcementData.title}</h1>
  {announcementData.date} <br />

  <hr />
  <div>{@html html}</div>
</main>

<img src="" alt="" />
