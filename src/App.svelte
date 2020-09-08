<script>
  import { tick } from "svelte";
  import Product from "./Product.svelte";
  import Modal from "./Modal.svelte";

  let showModal = false;
  let closeable = false;
  const toggleModal = e => {
    console.log("event: ", e);
    showModal = !showModal;
  };
  let products = [
    {
      id: "p-1",
      title: "A book",
      price: 9.99
    }
  ];

  let text = "This is some dummy text!";

  const addToCart = e => {
    console.log("e: ", e);
    console.log("detail: ", e.detail);
    console.log("id: ", e.detail.id);
  };
  const deleteProduct = e => {
    console.log("detail: ", e.detail);
  };

  const transform = e => {
    if (e.which !== 9) {
      return;
    }
    e.preventDefault();
    const selectionStart = e.target.selectionStart;
    const selectionEnd = e.target.selectionEnd;
    const value = e.target.value;

    text =
      value.slice(0, selectionStart) +
      value.slice(selectionStart, selectionEnd).toUpperCase() +
      value.slice(selectionEnd);

    tick().then(() => {
      e.target.selectionStart = selectionStart;
      e.target.selectionEnd = selectionEnd;
    });
  };
</script>

{#each products as product}
  <Product {...product} on:add-to-cart={addToCart} on:delete={deleteProduct} />
{/each}

<button on:click={toggleModal}>Show Modal</button>

{#if showModal}
  <!-- let assigns variable, you can assign to new variable, if not assigned, will default to named value
so default usable variable below would be "didAgree", but instead, we've assigned it to "closeable" -->
  <Modal
    on:cancel={toggleModal}
    on:close={toggleModal}
    let:didAgree={closeable}>
    <h1 slot="header">Hello there!</h1>
    <p>This works!</p>
    <button slot="footer" on:click={toggleModal} disabled={!closeable}>
      Confirm
    </button>
  </Modal>
{/if}

<textarea rows="5" value={text} on:keydown={transform} />
