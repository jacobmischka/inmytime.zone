<header>
	{#if date}
		<nav>
			<a href="/" on:click={handleClear}>Home</a>
		</nav>
	{/if}
</header>

<main>
	{#if date}
		<Date date={date} />
	{:else}
		<div>
			<p>Hello!</p>

			<p>
				Create a link to a time in your timezone,
				and it will automatically be converted to
				the local timezone of anyone who clicks on it.
			</p>
		</div>
	{/if}

	<Create />
</main>

<footer>
</footer>

<svelte:window on:popstate={getDate} />

<style>
	main {
		font-size: 1.25em;
		display: flex;
		flex-direction: column;
		justify-content: space-around;
		align-items: center;
	}

	div {
		padding: 2em;
	}
</style>

<script>
	import { DateTime } from 'luxon';

	import Date from './Date.svelte';
	import Create from './Create.svelte';

	const subRe = /(\d+)(am|pm|AM|PM)?/;

	let date;

	inspectSubdomain();
	getDate();


	function getDate() {
		try {
			const params = new URLSearchParams(window.location.search);
			let iso = params.get('iso');
			if (iso) {
				date = DateTime.fromISO(iso).setZone('local');
			}
		} catch (e) {
			console.error(e);
		}
	}

	function inspectSubdomain() {
		try {
			const { hostname, origin } = window.location;

			const hasSubdomain = hostname.indexOf('.') !== hostname.lastIndexOf('.');
			if (hasSubdomain) {
				let subdomain = hostname.substring(0, hostname.indexOf('.'));

				if (subdomain !== 'www') {
					const matches = subdomain.match(subRe);
					const time = matches[1];
					let hour = 0, minute = 0;

					if (time.length <= 2) {
						hour = Number(time);
					} else {
						const i = time.length - 2;
						hour = Number(time.substring(0, i));
						minute = Number(time.substring(i));
					}

					if (hour <= 12 && matches.length > 2) {
						const ampm = matches[2];
						if (ampm && ampm.toLowerCase() === 'pm') {
							hour += 12;
						}
					}

					const d = DateTime.local().set({ hour, minute });
					const domain = origin.replace(`${subdomain}.`, '');
					console.log(domain);
					const params = new URLSearchParams();
					params.set('iso', d.toISO());
					window.location =  `${domain}?${params.toString()}`;
				}
			}
		} catch (e) {
			console.error(e);
		}
	}

	function handleClear(event) {
		event.preventDefault();
		date = null;
		window.history.pushState(null, null, '/');
	}
</script>
