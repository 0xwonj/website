<script lang="ts">
  	import { goto } from '$app/navigation';
	import SpaceInvaders from '$lib/components/SpaceInvaders.svelte';
	import MoonPhase from '$lib/components/MoonPhase.svelte';

	let showGame = 0;
</script>


<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section>
	<h1>Wonjae's Website</h1>
	{#if showGame >= 3}
		<SpaceInvaders />
	{:else}
		<button class="MoonContainer" on:click={() => showGame++} aria-label="Show Moon Phase">
			<MoonPhase />
		</button>
	{/if}

	<main>
		<nav>
			<button class="link-button" on:click={() => goto('about')}>About</button>
			<button class="link-button" on:click={() => goto('archive')}>Archive</button>
			<button class="link-button" on:click={() => goto('connect')}>Connect</button>
		</nav>
	</main>
</section>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		margin: 3rem auto;
	}

	h1 {
		color: var(--color-theme-dynamic);
		animation: color-rotate 40s infinite ease-in-out alternate;
		margin: 2rem 0;
	}

	nav {
		margin-top: 2rem;
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 15px;
	}

	.link-button {
		background: none;
		border: none;
		color: var(--color-theme-dynamic);
		text-decoration: none;
		cursor: pointer;
		position: relative;
		padding-left: 20px;
		font-size: 1.5rem;
		padding: 1rem;
	}

	.link-button:hover {
		text-decoration: none;
		animation: blink 1s steps(1) infinite;
	}

	.link-button:hover::before {
		content: '>';
		position: absolute;
		left: -1rem;
		color: var(--color-theme-dynamic);
	}

	.MoonContainer {
		all: unset;
		cursor: pointer;
		margin: 2rem;
	}

	@keyframes color-rotate {
		0% {
			filter: hue-rotate(0deg);
		}
		100% {
			filter: hue-rotate(360deg);
		}
	}

	@media (max-width: 768px) {
		section {
			margin: 0 auto;

			h1 {
				margin: 1rem auto;
			}

			nav {
				margin: 1rem auto;
			}
		}
	}
</style>
