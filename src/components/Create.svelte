<form on:submit={handleSubmit}>
	<h2>Create a new link</h2>

	<label>
		Date and time
		<Flatpickr options={flatpickrOptions} bind:value={date} required />
	</label>

	<button type="submit">
		Submit
	</button>
</form>

<style>
	form {
	}
</style>

<script>
	import Flatpickr from 'svelte-flatpickr';

	let date = '';

	const flatpickrOptions = {
		enableTime: true,
		dateFormat: 'F j, Y h:i K'
	};

	function handleSubmit(event) {
		event.preventDefault();

		if (date) {
			const params = new URLSearchParams();
			params.set('iso', date.toISOString());
			window.history.pushState(null, null, `/?${params.toString()}`);
			window.dispatchEvent(new Event('popstate'));
		} else {
			alert('Enter a date');
		}
	}
</script>
