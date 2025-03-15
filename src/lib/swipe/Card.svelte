<script>
    export let companyName = '';
    export let jobDescription = '';
    export let imageUrl = '';
</script>

<div class="card border border-gray-300 rounded-lg p-4 max-w-xs text-center"></div>
    <img src={imageUrl} alt={companyName} class="w-full rounded-lg" />
    <h2 class="mt-4 mb-2 text-xl font-semibold">{companyName}</h2>
    <p class="m-0 text-gray-700">{jobDescription}</p>
</div>
