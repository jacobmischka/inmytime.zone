<header>
	<h1>inmytime.zone</h1>

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

	{#if !date}
		<aside>
			<p>
				Tip: type variations of the following into your address bar
				to easily create full links that you can send to others:
			</p>

			<ul>
				{#each examples as example}
					<li>
						<a href={`//${example}.inmytime.zone`}>{example}.inmytime.zone</a>
					</li>
				{/each}
			</ul>
		</aside>
	{/if}
</main>

<svelte:window on:popstate={getDate} />

<style>
	h1 {
		margin: 0;
	}

	main {
		font-size: 1.25em;
		display: flex;
		flex-direction: column;
		justify-content: space-around;
		align-items: center;
	}

	aside {
		text-align: center;
	}

	aside ul {
		padding: 0;
	}

	aside li {
		list-style: none;
	}
</style>

<script>
	import { DateTime } from 'luxon';

	import Date from './Date.svelte';
	import Create from './Create.svelte';

	const subRe = /(\d+)(am|pm|AM|PM)?/;

	const examples = [
		'10',
		'930',
		'12pm',
		'1034pm'
	];


	let date;


	const hasSubdomain = (
		window.location.hostname.indexOf('.')
		!== window.location.hostname.lastIndexOf('.')
	);

	if (hasSubdomain) {
		inspectSubdomain();
	}

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

				if (matches.length > 2 && matches[2]) {
					const ampm = matches[2].toLowerCase();
					if (hour < 12 && ampm === 'pm') {
						hour += 12;
					} else if (hour === 12 && ampm === 'am') {
						hour = 0;
					}
				}

				const d = DateTime.local().set({ hour, minute });
				const domain = origin.replace(`${subdomain}.`, '');
				console.log(domain);
				const params = new URLSearchParams();
				params.set('iso', d.toISO());
				window.location =  `${domain}?${params.toString()}`;
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
