<script>
    import { onMount } from 'svelte';
  
    export let day; // The selected day for the challenge
    let challenge = null;
  
    // Fetch the challenge JSON dynamically based on the day
    async function loadChallenge() {
      try {
        const response = await fetch(`/challenges/day${day}.json`);
        if (response.ok) {
          challenge = await response.json();
        } else {
          console.error('Challenge not found');
        }
      } catch (error) {
        console.error('Error loading challenge:', error);
      }
    }
  
    // Load the challenge when the component mounts or the day changes
    onMount(loadChallenge);
    $: if (day) loadChallenge();
  </script>
  
  {#if challenge}
    <div>
      <h2>Challenge for Day {challenge.day}</h2>
      <p>{challenge.instructions}</p>
      <p><strong>Hint:</strong> {challenge.hint}</p>
    </div>
  {:else}
    <p>Loading challenge...</p>
  {/if}
  