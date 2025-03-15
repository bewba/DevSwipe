<script>
    import Card from '$lib/components/swipe/Card.svelte';
</script>

<main class="text-center p-8 bg-gray-900 text-white min-h-screen flex flex-col items-center">

    <!-- Content -->
    <section class="flex flex-col items-center mt-6">
        <h1 class="text-4xl font-bold my-6 bg-gradient-to-r from-blue-400 to-cyan-300 text-transparent bg-clip-text mb-16">
            DevSwipe
        </h1>
        <Card companyName="Card Title" jobDescription="This is the content of the card." />
    </section>
</main>
