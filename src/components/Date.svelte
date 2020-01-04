{#if date && date.isValid}
	<date time={date.toISO()}>
		{date.toLocaleString(DateTime.DATETIME_FULL)}
	</date>
	<DateComparedToNow date={date} />
{:else}
	<p>Sorry, looks like the date was invalid.</p>
{/if}

<script>
	import { DateTime } from 'luxon';

	import DateComparedToNow from './DateComparedToNow.svelte';

	export let iso;
	let date;

	$: if (iso) {
		date = DateTime.fromISO(iso).setZone('local');
	}
</script>
