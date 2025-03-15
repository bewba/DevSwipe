<script>
	import { onMount } from 'svelte';

	let features = [
		{
			title: 'Swipe to Connect',
			description: 'Find and connect with tech jobs all over.'
		},
		{
			title: 'Project Collaboration',
			description: 'Easily discover and join projects and teams that match your skills.'
		},
		{
			title: 'Easy Application',
			description: 'Apply to companies easily with the help of a swipe.'
		}
	];
</script>

<main class="min-h-screen bg-gray-900 text-white flex flex-col items-center justify-center px-4">
	<section class="text-center">
		<h1 class="text-5xl font-bold mb-4">Welcome to DevSwipe</h1>
		<p class="text-lg text-gray-300 max-w-lg mx-auto mb-6">
			The ultimate platform to connect developers, collaborate on projects, and showcase your
			skills.
		</p>
		<a
			href="/signup"
			class="bg-blue-500 hover:bg-blue-600 text-white font-bold py-3 px-14 rounded-lg text-lg"
			>Get Started</a
		>
	</section>

	<section class="mt-16 grid grid-cols-1 md:grid-cols-3 gap-8 w-full max-w-4xl">
		{#each features as feature}
			<div class="bg-gray-800 p-6 rounded-lg shadow-lg text-center">
				<h2 class="text-xl font-semibold mb-2">{feature.title}</h2>
				<p class="text-gray-400">{feature.description}</p>
			</div>
		{/each}
	</section>
</main>
