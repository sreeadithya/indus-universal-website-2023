<script>
  import { db } from "../firebase";

  import { ref, set, get, child, remove } from "firebase/database";

  let departmentMembersData = { data: [], date: "", pinned: false, title: "" };

  let html = "";
  let blockType;
  let blockTag;
  let blockClosingTag;
  let tableRowClose;

  get(ref(db, "departmentMembers/")).then((snapshot) => {
    departmentMembersData = snapshot.val();

    for (let key in departmentMembersData.data.blocks) {
      blockType = departmentMembersData.data.blocks[key].type;
      console.log(blockType);

      if (blockType == "paragraph") {
        blockTag = "<p>";
        blockClosingTag = "</p>";
        html = html.concat(
          blockTag +
            departmentMembersData.data.blocks[key].data.text +
            blockClosingTag +
            "<br>"
        );
      }

      if (blockType == "header") {
        blockTag = `<h${departmentMembersData.data.blocks[key].data.level}>`;
        blockClosingTag = `</h${departmentMembersData.data.blocks[key].data.level}>`;
        html = html.concat(
          blockTag +
            departmentMembersData.data.blocks[key].data.text +
            blockClosingTag +
            "<br>"
        );
      }

      if (blockType == "List") {
        for (let itemKey in departmentMembersData.data.blocks[key].data.items) {
          blockTag = "<ul>";
          blockClosingTag = "</ul>";
          html = html.concat(
            blockTag +
              departmentMembersData.data.blocks[key].data.items[itemKey] +
              blockClosingTag +
              "<br>"
          );
        }
      }

      if (blockType == "table") {
        html = html.concat("<table>");
        if (departmentMembersData.data.blocks[key].data.withHeadings == true) {
          html = html.concat("<th><tr>");
        }
        for (let rowKey in departmentMembersData.data.blocks[key].data
          .content) {
          html = html.concat("<tr>");
          for (let elementKey in departmentMembersData.data.blocks[key].data
            .content[rowKey]) {
            if (
              departmentMembersData.data.blocks[key].data.withHeadings ==
                true &&
              rowKey == 0
            ) {
              html = html.concat(
                "<th>" +
                  departmentMembersData.data.blocks[key].data.content[rowKey][
                    elementKey
                  ] +
                  "</th>"
              );
            } else {
              html = html.concat(
                "<td>" +
                  departmentMembersData.data.blocks[key].data.content[rowKey][
                    elementKey
                  ] +
                  "</td>"
              );
            }
          }
          html = html.concat("</tr>");
        }

        if (departmentMembersData.data.blocks[key].data.withHeadings == true) {
          html = html.concat("</tr></th>");
        }

        html = html.concat("</table>");
      }
    }
  });
</script>

<main class="prose">
  <b>Last Updated: {departmentMembersData.lastUpdated}</b>
  <div>{@html html}</div>
</main>
