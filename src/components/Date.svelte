<section class="date">
	{#if date && date.isValid}
		<date time={date.toISO()}>
			{date.toLocaleString(DateTime.DATETIME_FULL)}
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
</script>
