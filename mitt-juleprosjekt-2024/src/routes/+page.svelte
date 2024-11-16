<script>
  import Card from '../lib/Card.svelte';

  // State to track flipped status of each card
  let flippedStates = Array(24).fill(false); // All cards are unflipped

  // Function to handle card flipping
  function handleFlip(index) {
    flippedStates[index] = !flippedStates[index]; // Toggle the flipped state
  }

  // Christmas countdown logic
  let today = new Date();
  let countDownDate = new Date(today.getFullYear(), 11, 25); // Set for December 25th

  if (today.getMonth() === 11 && today.getDate() > 25) {
    countDownDate.setFullYear(countDownDate.getFullYear() + 1);
  }

  let time_left_days;

  function updateCountdown() {
    const now = new Date();
    const one_day = 1000 * 60 * 60 * 24;
    const time_left_sec = countDownDate.getTime() - now.getTime();
    time_left_days = Math.ceil(time_left_sec / one_day);
  }

  // Update the countdown every second
  setInterval(updateCountdown, 1000);
  updateCountdown(); // Initial update
</script>

<div class="relative w-screen h-screen overflow-hidden">
  <!-- Christmas Countdown -->
  <div 
    class="
      absolute top-5 right-5 
      flex flex-col items-center
      rounded-full shadow-lg p-4 z-30
    "
  >
    <!-- Countdown Title -->
    <div class="text-white text-lg md:text-xl font-timer drop-shadow-lg">
      Dager til jul:
    </div>
    <!-- Countdown Number -->
    <div class="text-white text-2xl md:text-4xl font-timer drop-shadow-lg">
      {time_left_days}
    </div>
  </div>

  <!-- Video as background -->
  <video autoplay muted loop playsinline class="absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 min-w-full min-h-full object-cover z-0">
    <source src="/videos/fireplace.mp4" type="video/mp4">
    Din nettleser støtter ikke videoelementet.
  </video>

  <!-- Black overlay with gradient opacity for a modern fade effect -->
  <div class="absolute inset-0 bg-gradient-to-b from-black/70 via-black/50 to-black/70 z-10"></div>

  <!-- Content over video -->
  <div class="flex w-screen h-screen items-center justify-center flex-col relative z-20">
    <!-- Wrapper for header and grid, with vertical space distribution -->
    <div class="flex flex-col items-center justify-center gap-20 h-full py-20 z-20">
      <!-- Centered Header -->
      <h1 class="text-white font-holiday text-5xl md:text-6xl font-extrabold tracking-wide drop-shadow-lg">
        🎅 Julekalender 2024 🎄
      </h1>
    
      <!-- Grid of cards -->
      <div class="flex w-9/12 h-auto justify-center flex-wrap gap-4 items-center">
        {#each Array(24) as _, index}
          <Card day={index} 
              isFlipped={flippedStates[index]} 
              on:flip={() => handleFlip(index)} />
        {/each}
      </div>
    </div>
  </div>
</div>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Great+Vibes&family=Dancing+Script:wght@600&display=swap');

  /* Decorative holiday font for the header */
  .font-holiday {
    font-family: 'Great Vibes', cursive;
  }

  /* Countdown Timer Font */
  .font-timer {
    font-family: 'Dancing Script', cursive;
  }
</style>
