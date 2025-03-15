<script lang="ts">
    import Card from '$lib/components/swipe/Card.svelte';
    import { fade } from 'svelte/transition';

    // Sample job data - in a real app this would come from an API
    let jobs = [
        { id: 1, companyName: "Tech Corp", jobDescription: "Senior Frontend Developer" },
        { id: 2, companyName: "Startup Inc", jobDescription: "Full Stack Engineer" },
        { id: 3, companyName: "Big Tech", jobDescription: "React Developer" },
        { id: 4, companyName: "Innovation Labs", jobDescription: "Software Architect" }
    ];

    $: lastSwipeDirection = '';

    

    function handleCardRemove(event: CustomEvent<{ direction: 'left' | 'right' }>) {
        lastSwipeDirection = event.detail.direction;
        jobs = jobs.slice(1);
        console.log(lastSwipeDirection)
    }
</script>

<main class="text-center p-8 bg-gray-900 text-white min-h-screen flex flex-col items-center">
    <!-- Content -->
    <section class="flex flex-col items-center mt-6">
        <h1 class="text-4xl font-bold my-6 bg-gradient-to-r from-blue-400 to-cyan-300 text-transparent bg-clip-text mb-16">
            DevSwipe
        </h1>

        <div class="relative w-80 h-96">
            {#each jobs as job, i (job.id)}
                <div 
                    class="absolute top-0 left-0 w-full" 
                    style="
                        z-index: {jobs.length - i}; 
                        transform: scale({1 - i * 0.05}) translateY({i * 10}px);
                        filter: brightness({1 - i * 0.15});
                    "
                    transition:fade={{ duration: 200 }}
                >
                    <Card 
                        companyName={job.companyName}
                        jobDescription={job.jobDescription}
                        on:remove={handleCardRemove}
                    />
                </div>
            {/each}
            
            {#if jobs.length === 0}
                <div 
                    class="flex items-center justify-center w-full h-full border-2 border-dashed border-gray-600 rounded-lg"
                    transition:fade
                >
                    <p class="text-gray-400">No more jobs to show!</p>
                </div>
            {/if}
        </div>
    </section>
</main>

<style>
    .absolute {
        transition: transform 0.3s ease-out, filter 0.3s ease-out;
    }
</style>
