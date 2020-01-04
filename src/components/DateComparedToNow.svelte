<p>{diffStr} {inPast ? 'ago' : 'from now'}</p>

<script>
	import { DateTime } from 'luxon';

	const units = ['years', 'months', 'days', 'hours', 'minutes'];

	export let date;

	let now = DateTime.local(), diff, diffStr, inPast;

	$: inPast = date < now;
	$: diff = inPast ? now.diff(date, units) : date.diff(now, units);
	$: diffStr = formatDiff(diff);

	window.setInterval(() => {
		now = DateTime.local();
	}, 1000);

	function formatDiff(diff) {
		const pieces = [];
		for (let [unit, num] of Object.entries(diff.toObject())) {
			num = Math.floor(num);
			if (num) {
				if (num === 1 && unit.endsWith('s')) {
					unit = unit.substring(0, unit.length - 1);
				}

				pieces.push(`${num} ${unit}`);
			}
		}

		return pieces.join(' ');
	}
</script>
