<script lang="ts">
	import favicon from '$lib/assets/favicon.svg';
	import { resolve } from '$app/paths';

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

<nav>
	<div class="nav-container">
		<a href={resolve('/')} class="logo" data-sveltekit-preload-data="hover">🃏 PokerScrum</a>
		<div class="nav-links">
			<a href={resolve('/rooms')} data-sveltekit-preload-data="hover">Rooms</a>
			<a href={resolve('/about')} data-sveltekit-preload-data="hover">About</a>
		</div>
	</div>
</nav>

<main>
	{@render children?.()}
</main>

<style>
	:global(body) {
		margin: 0;
		font-family:
			-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans',
			'Helvetica Neue', sans-serif;
	}

	nav {
		background-color: #2c3e50;
		padding: 1rem 0;
		box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
	}

	.nav-container {
		max-width: 1200px;
		margin: 0 auto;
		padding: 0 2rem;
		display: flex;
		justify-content: space-between;
		align-items: center;
	}

	.logo {
		color: white;
		text-decoration: none;
		font-size: 1.5rem;
		font-weight: bold;
	}

	.nav-links {
		display: flex;
		gap: 2rem;
	}

	.nav-links a {
		color: white;
		text-decoration: none;
		transition: color 0.3s;
	}

	.nav-links a:hover {
		color: #4caf50;
	}

	main {
		min-height: calc(100vh - 60px);
	}
</style>
