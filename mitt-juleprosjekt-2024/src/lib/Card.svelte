<script>
  export let day;
  export let isFlipped = false; // Determines if the card is flipped

  import { createEventDispatcher } from 'svelte';
  const dispatch = createEventDispatcher();

  let showBack = isFlipped; // Initialize based on the flipped state

  // Reactive statement to update `showBack` based on `isFlipped`
  $: {
    if (isFlipped) {
      showBack = true; // Show the back card immediately when flipped
    } else {
      // Delay hiding the back card for animation
      setTimeout(() => {
        if (!isFlipped) showBack = false;
      }, 1000); // Adjust delay as needed to match animation
    }
  }

  function flip() {
    dispatch('flip'); // Notify the parent about the flip
  }
</script>

<!-- Button wrapper -->
<button 
  on:click={flip} 
  class="
    relative 
    w-32 h-32 
    group
    rounded-lg

  "
  style="
    overflow: hidden;
    perspective: 800px; /* Add depth for hinge effect */

  "
>
  <!-- Card container -->
  <div
    class="
      absolute inset-0
      transition-transform duration-[1s] 
      group-hover:scale-105
      rounded-lg
    "
    style="
      transform-origin: left; /* Set rotation origin */
      transform: rotateY({isFlipped ? -120 : 0}deg); /* Open the hatch */
      box-shadow: 0 0 15px rgba(255, 255, 255, 0.8); /* Soft glow around card */
      
    "
  >
    <!-- Front of the card -->
    <div
      class="
        absolute inset-0 
        bg-gradient-to-br from-red-400 to-red-600 
        flex items-center justify-center 
        rounded-lg shadow-lg
        text-black font-bold text-2xl
      "
      style="
        background-image: url('/textures/paper-texture.jpg');
        background-size: cover;
        background-repeat: no-repeat;
        background-blend-mode: multiply;
        border: 2px solid gold;
        box-shadow: 0 0 25px rgba(255, 0, 0, 0.6), 0 0 15px rgba(255, 215, 0, 0.5); /* Festive glow */
      "
    >
      <span class="leading-none font-holiday text-4xl">{day + 1}</span>
    </div>
  </div>

  <!-- Back of the card -->
  {#if showBack}
    <div
      class="
        absolute inset-0
        bg-gradient-to-br from-green-400 to-green-600 
        flex items-center justify-center
        rounded-lg shadow-inner
      "
      style="
        z-index: -1; /* Ensure it stays behind the front card */
        border: 2px solid gold;
        box-shadow: 0 0 20px rgba(0, 255, 0, 0.5); /* Subtle glow for the back */
      "
    >
      <div
        class="
          w-5/6 h-5/6 
          overflow-hidden rounded-lg
          flex items-center justify-center
        "
      >
        <img
          src={`/images/day${day + 1}.png`}
          alt={`Day ${day + 1}`}
          class="w-full h-full object-cover rounded-md "
        />
      </div>
    </div>
  {/if}
</button>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Great+Vibes&display=swap');

  /* Decorative holiday font for the card numbers */
  .font-holiday {
    font-family: 'Great Vibes', cursive;
  }
</style>
