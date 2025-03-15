<script lang="ts">
  import { createEventDispatcher } from 'svelte';

  let startX = 0;
  let startY = 0;
  let currentX = 0;
  let currentY = 0;
  let card: HTMLElement;
  let isDragging = false;
  let swipeThreshold = 100;
  let opacity = 1;
  const dispatch = createEventDispatcher();

  export let companyName = '';
  export let jobDescription = '';
  export let imageUrl = 'https://via.placeholder.com/300';

  function handleStart(event: TouchEvent | MouseEvent) {
    startX = event instanceof TouchEvent ? event.touches[0].clientX : (event as MouseEvent).clientX;
    startY = event instanceof TouchEvent ? event.touches[0].clientY : (event as MouseEvent).clientY;
    currentX = startX;
    currentY = startY;
    isDragging = true;
    card.style.transition = 'none';
  }

  function handleMove(event: TouchEvent | MouseEvent) {
    if (!isDragging) return;
    
    event.preventDefault();
    currentX = event instanceof TouchEvent ? event.touches[0].clientX : (event as MouseEvent).clientX;
    currentY = event instanceof TouchEvent ? event.touches[0].clientY : (event as MouseEvent).clientY;
    
    const diffX = currentX - startX;
    const diffY = currentY - startY;
    const rotation = diffX * 0.1;
    
    opacity = Math.max(1 - Math.abs(diffX) / (swipeThreshold * 2), 0.5);
    
    card.style.transform = `translate(${diffX}px, ${diffY}px) rotate(${rotation}deg)`;
    card.style.opacity = opacity.toString();
  }

  function handleEnd() {
    if (!isDragging) return;
    isDragging = false;
    
    const diffX = currentX - startX;
    card.style.transition = 'all 0.3s ease-out';
    
    if (Math.abs(diffX) > swipeThreshold) {
      const direction = diffX > 0 ? 'right' : 'left';
      card.style.transform = `translateX(${diffX > 0 ? window.innerWidth : -window.innerWidth}px) rotate(${diffX > 0 ? 30 : -30}deg)`;
      card.style.opacity = '0';
      setTimeout(() => dispatch('remove', { direction }), 300);
    } else {
      card.style.transform = 'translate(0, 0) rotate(0deg)';
      card.style.opacity = '1';
    }
  }

  function handleMouseLeave() {
    if (isDragging) {
      handleEnd();
    }
  }
</script>

<div
  bind:this={card}
  class="card border border-gray-300 rounded-lg p-4 w-80 h-96 text-center hover:shadow-xl hover:border-blue-500 hover:cursor-grab active:cursor-grabbing"
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
    user-select: none;
    touch-action: none;
    position: relative;
    background: white;
    will-change: transform, opacity;
  }
</style>