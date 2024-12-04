<svelte:head>
	<title>About</title>
	<meta name="description" content="About Wonjae Choi" />
</svelte:head>

<script>
	import profile from '$lib/images/profile.png';
</script>

<div class="about">
	<h1>About</h1>

	<div class="profile">
		<img src={profile} alt="Wonjae Choi" />
		<ul>
			<li>Wonjae Choi</li>
			<li>Ph.D Student at <a href="https://altair.snu.ac.kr/" target="_blank" rel="noopener">Seoul National University</a></li>
			<li>Blockchain Research Engineer at <a href="https://www.theradius.xyz/" target="_blank" rel="noopener">Radius</a></li>
			<li>Senior Researcher at <a href="https://x.com/DecipherGlobal/" target="_blank" rel="noopener">Decipher</a></li>	
	</div>

	<hr />

	<div class="research-interests">
		<ul class="list">
			<h2>Research Interests</h2>

			<li>Blockchain</li>
			<ul class="sub-list">
				<li>Ethereum</li>
				<li>Layer 2</li>
				<li>MEV</li>
			</ul>
			
			<li>Cryptography</li>
			<ul class="sub-list">
				<li>Zero-Knowledge Proof</li>
			</ul>
			<li>AI + Blockchain</li>
			<ul class="sub-list">
				<li>Federated Learning</li>
				<li>zkML</li>
			</ul>
			<li>Privacy Preserving</li>
		</ul>
		<span class="space"></span>
		<ul class="list">
		  <h2>Language</h2>
		  <li>Rust</li>
		  <li>Solidity</li>
		  <li>Python</li>
		  <li>Korean</li>
		  <li>English</li>
		</ul>
	</div>

	<hr />

	<div class="cv">
		<a href="/pdfs/CV_WonjaeChoi.pdf" target="_blank" rel="noopener"><h2>Curriculum Vitae</h2></a>
		<div class="pdf-container">
			<embed class="pdf-container-A4" src="/pdfs/CV_WonjaeChoi.pdf" type="application/pdf" width="100%" height="100%">
		</div>
	</div>
</div>

<style>
	.about {
		max-width: 48rem;
		width: 100%;
		margin: 0 auto;
		display: flex;
		flex-direction: column;
		background-color: var(--color-bg-0);

		h1 {
			color: var(--color-theme-dynamic);
			margin-bottom: 4rem;
		}

		hr {
			border: none;
			height: 2px;
			background-color: var(--color-text-0);
			margin: 2rem 0;
		}

	}

	.profile {
		display: flex;
		font-size: 1.25rem;

		img {
			margin-left: 1rem;
			width: 30%;
			border-radius: 50%;
			object-fit: cover;
		}
		
		ul {
			margin-left: 1rem;
			list-style-type: none;
			color: var(--color-text-0);
			display: flex;
			flex-direction: column;
			gap: 2rem;

			li:before {
				content: '>';
				margin-right: 1rem;
				color: var(--color-theme-dynamic);
				font-family: var(--font-body);	
				position: relative;
				top: 4px;
			}
		}
	}

	.research-interests {
		display: flex;
		justify-content: space-between;

		.list {
			margin: 0;
			padding: 0;
			list-style-type: none;
			color: var(--color-text-0);
			display: flex;
			flex-direction: column;
			gap: 1rem;

			li {
				font-size: 1.25rem;
				font-weight: 550;
			}

			li:before {
				content: '>';
				margin-right: 1rem;
				color: var(--color-theme-dynamic);
				font-family: var(--font-body);
				position: relative;
				top: 4px;
			}
		}

		.sub-list {
			list-style-type: none;
			color: var(--color-text-0);
			li {
				font-size: 1rem;
				font-weight: 400;
			}

			li:before {
				content: '⦁';
				margin-right: 1rem;
				color: var(--color-theme-dynamic);
				position: relative;
				top: 2px;
			}
		}
	}

	.cv {
		h2 {
			text-align: center;
			margin-bottom: 3rem;
		}

		a:hover {
			text-decoration: none;
			opacity: 0.5;
		}

		.pdf-container {
			width: 100%;
			aspect-ratio: 210 / 310;			
		}
	}

	@media (max-width: 768px) {
		.about {
			h1 {
				margin: 0 0 2rem 0;
			}
		}
		.profile {
			display: flex;
			flex-direction: column;
			align-items: center;
			justify-content: center;

			img {
				width: 60%;
				margin: 0;
			}

			ul {
				padding: 0;
				margin: 2rem 1rem 1rem 1rem;
			}
		}

		.research-interests {
			flex-direction: column;
			
			h2 {
				text-align: center;
			}

			.list {
				margin: 0 2rem 1rem 2rem;
			}
		}
	}
</style>