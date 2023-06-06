<script lang="ts">
    import Eye from "$lib/Eye.svelte";
    import { onMount } from "svelte";

    let clientX = 0;
    let clientY = 0;
    onMount(() => {
        document.addEventListener("mousemove", (e) => {
            clientX = e.clientX;
            clientY = e.clientY;
        });
    });
</script>

<!-- {#each [...Array(400).keys()] as k}
<div style="position:absolute;top:{(200*(Math.floor(k/20)))}px;left:{200*(k%20)}px">
    <Eye bind:clientX bind:clientY/>
    </div>
{/each} -->

<!-- <div style="position:absolute;top:{300}px;left:{300}px">
    <Eye bind:clientX bind:clientY/>
</div> -->

<div style="position:absolute;top:{300}px;left:{300}px">
    <Eye bind:clientX bind:clientY/>
</div>
<div style="position:absolute;top:{300}px;left:{480}px">
    <Eye bind:clientX bind:clientY/>
</div>