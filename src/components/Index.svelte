{#if error}
	<p>Sorry, there was a problem.</p>
{:else if date && date.isValid}
<date time={date.toISO()}>
	{date.toLocaleString(DateTime.DATETIME_FULL)}
</date>
{:else if date}
	<p>Sorry, looks like the date was invalid.</p>
{:else}
	<p>Hey!</p>
{/if}

<script>
	import { DateTime } from 'luxon';

	let date, error;

	try {
		const params = new URLSearchParams(window.location.search);
		const iso = params.get('iso');
		if (iso) {
			date = DateTime.fromISO(iso).setZone('local');
		}
	} catch (e) {
		console.error(e);
		error = e;
	}
</script>
