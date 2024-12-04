<script lang="ts">
	import logo from '$lib/images/star.svg';
</script>

<header>
	<div class="corner">
		<a href="/">
			<img src={logo} alt="Yellow Star" />
		</a>
	</div>
</header>

<style>
	header {
		display: flex;
		justify-content: space-between;
		height: 3rem;
	}

	.corner {
		width: 2.5rem;
		height: 2.5rem;
	}

	.corner a {
		display: flex;
		align-items: center;
		justify-content: center;
		width: 100%;
		height: 100%;
	}

	.corner img {
		width: 1.5rem;
		height: 1.5rem;
		object-fit: contain;
	}

	.corner img:hover {
		transform: rotate(36deg);
	}
</style>
