<script>
  import { createEventDispatcher } from "svelte";

  export let title;
  export let price;
  export let bestseller = false; //assign default value for optional prop
  export let id;

  const dispatch = createEventDispatcher();

  const addToCart = () => {
    dispatch("add-to-cart", { id });
  };
  const deleteProduct = () => {
    dispatch("delete", id);
  };
</script>

<style>

</style>

<article>
  <h1>{title}</h1>
  <h2>${price}</h2>
  {#if bestseller}
    <h3>BESTSELLER!</h3>
  {/if}
  <button on:click={addToCart}>Add to Cart</button>
  <button on:click={deleteProduct}>Delete</button>
</article>
