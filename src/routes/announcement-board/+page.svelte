<script>
  const myobject = {
    time: 1693408945761,
    blocks: [
      {
        type: "paragraph",
        data: {
          text: 'zxclkasjdklasdaasdas <a href="http://rian.com">aasdmashd</a>',
        },
      },
      {
        type: "paragraph",
        data: {
          text: "<i>as</i>",
        },
      },
      {
        type: "paragraph",
        data: {
          text: "<b>dasdjashdkashdas</b>",
        },
      },
    ],
    version: "2.19.3",
  };

  let html = "";

  for (let key in myobject.blocks) {
    html = html.concat(myobject.blocks[key].data.text + "<br>");
  }
</script>

<main>
  {@html html}
</main>
