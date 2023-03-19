<script lang="ts">
	import { getContext, onDestroy, onMount } from 'svelte';
	import type { CanvasContext, DrawFn } from '../types';
	export let src: string = '/bracket_blank.png';

	let draw: DrawFn;

	let canvasContext = getContext('canvas') as CanvasContext;

	onMount(() => {
		const background = new Image();
		background.src = src;

		draw = (ctx: CanvasRenderingContext2D) => {
			ctx.drawImage(background, 0, 0);
		};
		canvasContext.addDrawFn(draw);
	});

	onDestroy(() => {
		canvasContext.removeDrawFn(draw);
	});
</script>
