<script lang="ts">
	import { onMount } from 'svelte';

	let starCount: number;
	const starColors = ['#fff', '#ffdd88', '#88bbff', '#ffffff88', '#ff8888', '#88ff88', '#8888ff', '#ff88ff', '#ffff88'];

	function calculateStarCount() {
		return Math.floor((window.innerWidth * window.innerHeight) / 8192); // / 8192로 가독성 향상
	}

	function createStars() {
		starCount = calculateStarCount();

		const stars = Array(starCount)
			.fill(0)
			.map(() => {
				const x = Math.random() * window.innerWidth;
				const y = Math.random() * window.innerHeight;
				const color = starColors[Math.floor(Math.random() * starColors.length)];
				return `${x}px ${y}px ${color}`;
			});

		const starsElement = document.querySelector('.stars') as HTMLElement;
		if (starsElement) {
			starsElement.style.setProperty('--star-box-shadow', stars.join(', '));
		}
	}

	onMount(() => {
		createStars();
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
