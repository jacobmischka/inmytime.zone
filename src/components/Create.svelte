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
	h2 {
		font-size: 1.25em;
	}
</style>

<script>
	import Flatpickr from 'svelte-flatpickr';

	const MINUTE_ROUND_TIME = 15;

	let date = '';

	let defaultHour, defaultMinute;

	try {
		const now = new Date();
		defaultHour = now.getHours();
		defaultMinute = now.getMinutes() + MINUTE_ROUND_TIME;
		defaultMinute -= defaultMinute % MINUTE_ROUND_TIME;

		if (defaultMinute > 60) {
			defaultMinute -= 60;
			defaultHour += 1;
		}

		if (defaultHour > 24) {
			defaultHour = undefined;
			defaultMinute = undefined;
		}
	} catch (e) {
		console.error(e);
	}



	const flatpickrOptions = {
		enableTime: true,
		dateFormat: 'F j, Y h:i K',
		defaultHour,
		defaultMinute,
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
