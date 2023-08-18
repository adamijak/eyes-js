<script lang="ts">
    import { onMount } from "svelte";
    import * as vec from "$lib/vec";
    export let clientX = 0;
    export let clientY = 0;

    let box_x = 0;
    let box_y = 0;
    let cx = 0.01;
    let cy = 0.01;

    let xdeg = 0;
    let ydeg = 0;

    let sclera = {
        cx: 100,
        cy: 100,
        r: 80,
    };

    let iris = {
        cx: 100,
        cy: 100,
        r: 40,
        style:"",
    };

    let pupil = {
        cx: 100,
        cy: 100,
        r: 20,
        style:"",
    };

    const mult = 180 / Math.PI;


    let svg: SVGSVGElement | null = null;
    onMount(() => {
        box_x = svg?.getBoundingClientRect().x ?? 0;
        box_y = svg?.getBoundingClientRect().y ?? 0;
        cx = box_x + sclera.cx;
        cy = box_y + sclera.cy;
    });

    $: {
        let dx = clientX - cx;
        let dy = clientY - cy;
        const norm = vec.getNorm(dx, dy);
        const dist = 1000;
        xdeg = Math.atan(dy/dist) * mult;
        ydeg = Math.atan(dx/dist) * mult;
        pupil.r = Math.min(20, Math.max(10, 20-(10 * Math.sqrt(norm))/50));

    }
</script>

<svg bind:this={svg}>
    <circle class="sclera" {...sclera} />
    <circle class="iris" {...iris} style="--xdeg:{xdeg}deg; --ydeg:{-ydeg}deg;" />
    <circle class="pupil" {...pupil} style="--xdeg:{xdeg}deg; --ydeg:{-ydeg}deg;" />
</svg>

<style>
    svg {
        width: 200px;
        height: 200px;
    }
    .sclera {
        stroke: black;
        fill: white;
        stroke-width: 2;
    }

    .iris {
        stroke: black;
        stroke-width: 1;
        fill: brown;
        transform-origin: center center 80px;
        transform: rotateY(var(--ydeg)) rotateX(var(--xdeg));
    }

    .pupil {
        fill: black;
        transform-origin: center center 80px;
        transform: rotateY(var(--ydeg)) rotateX(var(--xdeg)) translateZ(-10px);        
    }
</style>
