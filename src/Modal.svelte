<script>
  // your script goes here
  import { blur, fade, fly, slide, scale } from 'svelte/transition';
</script>

<style>
  /* your styles go here */

  .modal-wrapper {
    position: fixed;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    background: rgba(0, 0, 0, 0.8);
  }
  .modal {
    background: white;
    padding: 20px;
    border-radius: 15px;
  }
</style>

<div class="modal-wrapper" transition:fade>

  <div class="modal" transition:scale>
    <button>Close</button>
    <!-- <slot> optional fallback </slot> -->
    <slot />
    <!-- <slot name="modal" /> -->
  </div>
</div>
