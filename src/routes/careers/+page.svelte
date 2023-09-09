<script>
  let showModal = false;
  let dialog;
  $: if (dialog && showModal) dialog.showModal();

  import { Notyf } from "notyf";
  import "notyf/notyf.min.css";
  var notyf = new Notyf({
    duration: 2000,
  });
</script>

<main>
  <button>download cv format</button> <br />

  Eligible candidates are requested to download and fill the form and send it to

  <button
    on:click={() => {
      showModal = true;
    }}
  >
    careers@indusuniversalschool.com
  </button>. For any further information, please feel free to contact us at the
  same email.

  <!-- ignore all errors its just a11y crying and whining about some accessability errors -->

  <dialog
    bind:this={dialog}
    on:close={() => (showModal = false)}
    on:click|self={() => dialog.close()}
  >
    <div on:click|stopPropagation>
      <h2 class="text-lg">email options</h2>
      <ol>
        <li>
          <button>
            <a
              href="https://mail.google.com/mail/u/0/?fs=1&tf=cm&source=mailto&to=careers@indusuniversalschool.com"
              target="_blank">open in gmail</a
            ></button
          >
        </li>
        <li>
          <button>
            <a
              href="https://outlook.live.com/mail/0/deeplink/compose/?to=careers@indusuniversalschool.com"
              target="_blank">open in outlook</a
            ></button
          >
        </li>
        <li>
          <button>
            <a
              href="https://compose.mail.yahoo.com/?to=careers%40indusuniversalschool.com"
              target="_blank">open in yahoo</a
            ></button
          >
        </li>
        <button
          on:click|preventDefault={() => {
            showModal = false;
            navigator.clipboard.writeText("careers@indusuniversalschool.com");
            notyf.success("email copied to clipboard");
            dialog.close();
          }}
        >
          copy email
        </button>
        <button autofocus on:click={() => dialog.close()}>close modal</button>
      </ol>
    </div>
  </dialog>
</main>

<style>
  dialog {
    max-width: 32em;
    border-radius: 0.2em;
    border: none;
    padding: 0;
  }
  dialog::backdrop {
    background: rgba(0, 0, 0, 0.3);
  }
  dialog > div {
    padding: 1em;
  }
  dialog[open] {
    animation: zoom 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
  }
  @keyframes zoom {
    from {
      transform: scale(0.95);
    }
    to {
      transform: scale(1);
    }
  }
  dialog[open]::backdrop {
    animation: fade 0.2s ease-out;
  }
  @keyframes fade {
    from {
      opacity: 0;
    }
    to {
      opacity: 1;
    }
  }
  button {
    display: block;
  }
</style>
