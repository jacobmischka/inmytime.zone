<Router>
	<nav>
		<Link to="create">Create a new link</Link>
	</nav>
	<main>
		<Route path="*" component={Index} />
		<Route path="create" component={Create} />
	</main>
</Router>

<script>
	import { Router, Route, Link } from 'svelte-routing';
	import Index from './Index.svelte';
	import Create from './Create.svelte';
</script>
