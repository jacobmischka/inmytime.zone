<div>
	{#if iso}
		<Date iso={iso} />
	{:else}
		<p>Hello!</p>

		<p>
			Create a link to a time in your timezone,
			and it will automatically be converted to
			the local timezone of anyone who clicks on it.
		</p>
	{/if}
</div>

<style>
	div {
		text-align: center;
	}
</style>

<script>
	import Date from './Date.svelte';


	let iso;

	try {
		const params = new URLSearchParams(window.location.search);
		iso = params.get('iso');
	} catch (e) {
		console.error(e);
	}
</script>
