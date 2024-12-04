<script lang="ts">
	import { onMount } from 'svelte';
	import { get } from 'svelte/store';
	import { page } from '$app/stores';

	let starCount: number;
	const starColors = ['#fff', '#ffdd88', '#88bbff', '#ffffff88', '#ff8888', '#88ff88', '#8888ff', '#ff88ff', '#ffff88'];
	let currentUrl: string;

	function calculateStarCount() {
		const pageWidth = document.documentElement.scrollWidth;
		const pageHeight = document.documentElement.scrollHeight * 1.1;
		return Math.floor((pageWidth * pageHeight) / 8192);
	}

	function createStars() {
		starCount = calculateStarCount();

		const stars = Array(starCount)
			.fill(0)
			.map(() => {
				const x = Math.random() * document.documentElement.scrollWidth;
				const y = Math.random() * document.documentElement.scrollHeight * 1.1;
				const color = starColors[Math.floor(Math.random() * starColors.length)];
				return `${x}px ${y}px ${color}`;
			});

		const starsElement = document.querySelector('.stars') as HTMLElement;
		if (starsElement) {
			starsElement.style.setProperty('--star-box-shadow', stars.join(', '));
		}
	}

	function handlePageChange() {
		const newUrl = get(page).url.pathname;
		if (newUrl !== currentUrl) {
			currentUrl = newUrl;
			createStars();
		}
	}

	onMount(() => {
		currentUrl = get(page).url.pathname;
		createStars();

		page.subscribe(() => handlePageChange());
	});
</script>

<div class="stars"></div>

<style>
	.stars {
		width: 2px;
		height: 2px;
		background: transparent;
		position: absolute;
		top: 0;
		left: 0;
		z-index: -1;
		box-shadow: var(--star-box-shadow);
		animation: twinkle 2s infinite ease-in-out alternate;
	}

	@keyframes twinkle {
		0% {
			opacity: 0.5;
			filter: hue-rotate(0deg);
		}
		50% {
			opacity: 1;
			filter: hue-rotate(30deg);
		}
		100% {
			opacity: 0.5;
			filter: hue-rotate(60deg);
		}
	}
</style>
