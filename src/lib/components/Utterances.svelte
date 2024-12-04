<script lang="ts">
    import { onMount } from "svelte";

    export let repo: string;
    export let theme: string = "github-dark";
    export let issueTerm: string = "pathname";
    export let label: string = "comment";

    let container: HTMLElement | null = null;

    onMount(() => {
        if (!repo) {
            console.error("Utterances: 'repo' prop is required.");
            return;
        }

        if (container?.hasChildNodes()) {
            return;
        }

        const script = document.createElement("script");
        script.src = "https://utteranc.es/client.js";
        script.async = true;
        script.crossOrigin = "anonymous";
        script.setAttribute("repo", repo);
        script.setAttribute("issue-term", issueTerm);
        script.setAttribute("theme", theme);
        script.setAttribute("label", label);

        container?.appendChild(script);
    });
</script>
  
<div bind:this={container}></div>
  