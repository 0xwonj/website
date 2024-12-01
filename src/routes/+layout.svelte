<script lang="ts">
	import Stars from './Stars.svelte';
	import Header from './Header.svelte';
	import { onMount } from 'svelte';
	import '../app.css';

	let { children } = $props();

	const dayColors: Record<number, string> = {
		0: '#FF6347',
		1: '#FF7F00',
		2: '#FFD700',
		3: '#32CD32',
		4: '#1E90FF',
		5: '#8A2BE2',
		6: '#FF69B4'
	};

	const today = new Date().getDay();
	const dynamicColor = dayColors[today];

	onMount(() => {
		document.documentElement.style.setProperty('--color-theme-dynamic', dynamicColor);
	});

</script>

<div class="app">
	<Stars />
	<Header />
	<main>
		{@render children()}
	</main>
</div>

<style>
	.app {
		display: flex;
		flex-direction: column;
		/* width: 100%; */
		min-height: 100vh;
		overflow-x: hidden;
	}

	main {
		flex: 1;
		display: flex;
		flex-direction: column;
		padding: 1rem;
		width: 100%;
		max-width: 64rem;
		margin: 0 auto;
		box-sizing: border-box;
	}
</style>
