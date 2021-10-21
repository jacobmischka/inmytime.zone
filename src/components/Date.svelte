<section class="date">
	{#if date && date.isValid}
		<date time={date.toISO()}>
			{date.toLocaleString(format)}
		</date>
		<DateComparedToNow date={date} />
	{:else}
		<p>Sorry, looks like the date was invalid.</p>
	{/if}
</section>

<style>
	section {
		text-align: center;
	}

	date {
		font-size: 1.25em;
		font-weight: bold;
	}
</style>

<script>
	import { DateTime } from 'luxon';

	import DateComparedToNow from './DateComparedToNow.svelte';

	export let date;
	export let timeOnly = false;

	let format;
	$: format = timeOnly ? DateTime.TIME_SIMPLE : DateTime.DATETIME_FULL;
</script>
