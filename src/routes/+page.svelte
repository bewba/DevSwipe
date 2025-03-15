<script>
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
	<!-- HEADER -->
	<section class="text-center">
		<h1 class="text-5xl font-bold mb-4 bg-gradient-to-r from-blue-400 to-cyan-400 text-transparent bg-clip-text">
			Welcome to DevSwipe
		</h1>
		<p class="text-lg text-gray-300 max-w-lg mx-auto mb-6">
			The ultimate platform to connect developers, collaborate on projects, and showcase your skills.
		</p>
		<a
			href="/signup"
			class="bg-gradient-to-r from-blue-500 to-cyan-500 hover:from-blue-600 hover:to-cyan-600 
			text-white font-bold py-3 px-14 rounded-lg text-lg shadow-lg transform transition-all duration-300 hover:scale-105"
		>
			Get Started
		</a>
	</section>

	<!-- FEATURES -->
	<section class="mt-16 grid grid-cols-1 md:grid-cols-3 gap-8 w-full max-w-5xl">
		{#each features as feature}
			<div class="bg-gray-800 p-6 rounded-xl shadow-lg text-center border border-gray-700 hover:border-cyan-400 transition-all duration-300">
				<h2 class="text-xl font-semibold text-cyan-300 mb-2">{feature.title}</h2>
				<p class="text-gray-400">{feature.description}</p>
			</div>
		{/each}
	</section>
</main>
