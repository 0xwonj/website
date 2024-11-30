<svelte:head>
	<title>{data.meta.title}</title>
	<meta property="og:type" content="article" />
	<meta property="og:title" content={data.meta.title} />
</svelte:head>

<script lang="ts">
	import { formatDate } from '$lib/utils'
    import { Utterances } from "@codewithshin/svelte-utterances"
	let { data } = $props()
</script>

<article>
	<div class="prose">
        <h1>{data.meta.title}</h1>
        <div class="metadata">
            <p>{formatDate(data.meta.date)}</p>
            <a href="/archive" class="back-link">See all posts</a>
        </div>
		<data.content />
	</div>
</article>

<div class="comments">
    <hr />
    <Utterances reponame="0xwonj/comments" issueTerm="pathname" label="comment" theme="photon-dark" />
</div>

<style>
    article {
        all: unset;
        max-width: 48rem;
        width: 100%;
        margin: 1rem auto;
        background-color: var(--color-bg-0);
        font-family: var(--font-text);
    }
    
    .metadata {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 3rem;
        font-size: 0.8rem;

        a {
            color: var(--color-theme-dynamic);
            text-decoration: none;
        }

        a:hover {
            color: var(--color-theme-dynamic);
            text-decoration: underline;
        }

        p {
            margin: 0.5rem 0;
            color: #8b8b8b;
        }
    }

    .comments {
        max-width: 48rem;
        width: 100%;
        margin: 2rem auto;
    }
</style>
