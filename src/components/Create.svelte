<form on:submit={handleSubmit}>
	<h2>Create a new link</h2>

	<div>
		<label>
			{#if timeOnly}
				Time
			{:else}
				Date and time
			{/if}

			{#key options}
			<Flatpickr {options} bind:value={date} required />
			{/key}
		</label>

		<button type="submit">
			Submit
		</button>
	</div>

	<div>
		<label>
			<input type="checkbox" bind:checked={timeOnly} />
			Time only
		</label>
	</div>
</form>

<style>
	h2 {
		font-size: 1.25em;
	}

	input {
		display: inline-block;
	}
</style>

<script>
	import Flatpickr from 'svelte-flatpickr';
	import { DateTime } from 'luxon';

	const MINUTE_ROUND_TIME = 15;

	let date = '';
	let timeOnly = true;

	let defaultHour, defaultMinute;

	try {
		const now = new Date();
		defaultHour = now.getHours();
		defaultMinute = now.getMinutes() + MINUTE_ROUND_TIME;
		defaultMinute -= defaultMinute % MINUTE_ROUND_TIME;

		if (defaultMinute >= 60) {
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



	let options;
	$: options = {
		enableTime: true,
		noCalendar: timeOnly,
		dateFormat: timeOnly ? 'h:i K' : 'F j, Y h:i K',
		defaultHour,
		defaultMinute,
	};

	function handleSubmit(event) {
		event.preventDefault();

		if (date) {
			const params = new URLSearchParams();
			if (timeOnly) {
				const dt = DateTime.fromJSDate(date);
				params.set('time', dt.toFormat('HH:mm'));
				params.set('zone', dt.zoneName);
			} else {
				params.set('iso', date.toISOString());
			}
			window.history.pushState(null, null, `/?${params.toString()}`);
			window.dispatchEvent(new Event('popstate'));
		} else {
			alert('Enter a date');
		}
	}
</script>
