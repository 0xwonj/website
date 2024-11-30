<svelte:head>
	<title>Connect</title>
	<meta name="description" content="Social links" />
</svelte:head>

<script>
</script>

<div class="connect">
    <h1>Connect</h1>
    <ul>
        <hr />
        <li>
            <a href="https://github.com/0xwonj" class="link">
                Github
                <span class="tooltip">0xwonj</span>
            </a>
        </li>
        <hr />
        <li>
            <a href="mailto:wonjae@snu.ac.kr" class="link">
                Email
                <span class="tooltip">wonjae@snu.ac.kr</span>
            </a>
        </li>
        <hr />
        <li>
            <a href="https://twitter.com/0xwonj" class="link">
                Twitter
                <span class="tooltip">0xwonj</span>
            </a>
        </li>
        <hr />
        <li>
            <a href="https://linkedin.com/wonj" class="link">
                LinkedIn
            </a>
        </li>
        <hr />

</div>

<style>
    .connect {
        max-width: 48rem;
		width: 100%;
		margin: 1rem auto;
    }

    ul {
        list-style: none;
        margin: 4rem;
        justify-items: center;
        font-size: 2rem;
        padding: 0;
        display: flex;
        flex-direction: column;
        align-items: center;
        a {
            font-family: var(--font-body);
            color: var(--color-text-0);
        }

        a:hover {
            color: var(--color-theme-dynamic);
            text-decoration: none;
        }
    }

    hr {
        border: none;
        height: 2px;
        background-color: var(--color-theme-dynamic);
        margin: 2rem 0;
        width: 100%;
    }

    .link {
        position: relative;
        display: inline-block;
        color: var(--color-text-0);
    }

    .tooltip {
        visibility: hidden;
        opacity: 0;
        position: absolute;
        top: 50%;
        left: 110%;
        transform: translateY(-50%);
        background-color: var(--color-bg-0);
        color: var(--color-text-0);
        padding: 0.5rem 1rem;
        border-radius: 0.25rem;
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        white-space: nowrap;
        transition: opacity 0.3s ease, visibility 0.3s ease;
        font-size: 1rem;
        background-color: var(--color-theme-dynamic);
    }

    .link:hover .tooltip {
        visibility: visible;
        opacity: 1;
    }
</style>