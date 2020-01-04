<h1>Create a new link</h1>

<form on:submit={handleSubmit}>
	<Flatpickr options={flatpickrOptions} bind:value={date} required />

	<button type="submit">
		Submit
	</button>
</form>

<style>
	form {
		text-align: center;
	}
</style>

<script>
	import { navigate } from 'svelte-routing';
	import Flatpickr from 'svelte-flatpickr';

	let date = '';

	const flatpickrOptions = {
		enableTime: true
	};

	function handleSubmit(event) {
		event.preventDefault();

		if (date) {
			const params = new URLSearchParams();
			params.set('iso', date.toISOString());
			navigate(`/?${params.toString()}`);
		} else {
			alert('Enter a date');
		}
	}
</script>
