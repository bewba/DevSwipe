<script>
  import { createEventDispatcher } from 'svelte';

  let startX = 0;
  let card;
  let isDragging = false;
  const dispatch = createEventDispatcher();

  export let companyName = '';
  export let jobDescription = '';
  export let imageUrl = 'https://via.placeholder.com/300'; // Default image

  function handleStart(event) {
    startX = event.type === 'touchstart' ? event.touches[0].clientX : event.clientX;
    isDragging = true;
  }

  function handleMove(event) {
    if (!isDragging) return;
    let clientX = event.type === 'touchmove' ? event.touches[0].clientX : event.clientX;
    let diff = clientX - startX;
    card.style.transform = `translateX(${diff}px) rotate(${diff * 0.1}deg)`;
  }

  function handleEnd(event) {
    if (!isDragging) return;
    isDragging = false;
    let clientX = event.type === 'touchend' ? event.changedTouches[0].clientX : event.clientX;
    let diff = clientX - startX;
    if (diff > 50) {
      card.style.transform = 'translateX(100%) rotate(20deg)';
      card.style.opacity = '0';
      setTimeout(() => dispatch('remove'), 300); // Dispatch remove event after animation
    } else if (diff < -50) {
      card.style.transform = 'translateX(-100%) rotate(-20deg)';
      card.style.opacity = '0';
      setTimeout(() => dispatch('remove'), 300); // Dispatch remove event after animation
    } else {
      card.style.transform = '';
    }
  }

  function handleMouseLeave() {
    if (isDragging) {
      isDragging = false;
      card.style.transform = '';
    }
  }
</script>

<div
  bind:this={card}
  class="card border border-gray-300 rounded-lg p-4 w-80 h-96 text-center transition-transform duration-300 hover:shadow-xl hover:scale-105 hover:border-blue-500 hover:cursor-pointer"
  on:touchstart={handleStart}
  on:touchmove={handleMove}
  on:touchend={handleEnd}
  on:mousedown={handleStart}
  on:mousemove={handleMove}
  on:mouseup={handleEnd}
  on:mouseleave={handleMouseLeave}
>
  <img src={imageUrl} alt={companyName} class="w-full h-48 object-cover rounded-lg" />
  <h2 class="mt-4 mb-2 text-xl font-semibold">{companyName}</h2>
  <p class="m-0 text-gray-700 truncate">{jobDescription}</p>
</div>

<style>
  .card {
    max-width: 100%;
  }
</style>  