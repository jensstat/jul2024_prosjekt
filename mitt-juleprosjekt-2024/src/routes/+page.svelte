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

<div class="relative w-screen overflow-hidden">
  <!-- Video as background -->
  <video autoplay muted loop playsinline class="absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 min-w-full min-h-full object-cover z-0">
    <source src="/videos/fireplace.mp4" type="video/mp4">
    Din nettleser støtter ikke videoelementet.
  </video>

  <!-- Black overlay with gradient opacity for a modern fade effect -->
  <div class="absolute inset-0 bg-gradient-to-b from-black/70 via-black/50 to-black/70 z-10"></div>

  <!-- Content over video -->
  <div class="flex w-screen items-center justify-center flex-col relative z-20">
    <!-- Wrapper for header and grid, with vertical space distribution -->
    <div class="flex flex-col items-center justify-center gap-10 lg:gap-20 h-full py-10 lg:py-20 z-20">
      <!-- Centered Header -->
      <h1 class="text-white font-holiday text-4xl md:text-5xl lg:text-6xl font-extrabold tracking-wide drop-shadow-lg">
        🎅 Julekalender 2024 🎄
      </h1>

      <!-- Countdown Timer -->
      <div 
        class="
          flex flex-col items-center gap-2 bg-gradient-to-br 
          p-3 rounded-full shadow-lg z-30
          lg:absolute lg:top-5 lg:right-5 lg:static lg:mt-0 mt-5
          
        "
      >
        <!-- Countdown Title -->
        <div class="text-white text-2xl lg:text-2xl font-timer drop-shadow-lg ">
          Dager til jul:
        </div>
        <!-- Countdown Number -->
        <div class="text-white text-4xl lg:text-3xl font-timer drop-shadow-lg">
          {time_left_days}
        </div>
      </div>
    
      <!-- Grid of cards -->
      <div 
        class="
          grid grid-cols-3 sm:grid-cols-4 lg:grid-cols-6
          gap-2 sm:gap-4 lg:gap-6
          w-full max-w-screen-lg px-4
        "
      >
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
