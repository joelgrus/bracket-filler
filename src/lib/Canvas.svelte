<script lang="ts">
	import { onMount } from 'svelte';

	export let src = '/bracket_blank.png';
	export let width = 1878;
	export let height = 977;
	let canvas: HTMLCanvasElement;
	let downloadButton: HTMLButtonElement;

	let texts = new Array(16).fill('jupyter notebooks');

	type Loc = [number, number];

	function size(ctx: CanvasRenderingContext2D, text: string, fontSize: number) {
		ctx.font = `${fontSize}px Arial`;

		const lines = text.split('\n');
		const widths = lines.map((line) => ctx.measureText(line).width);
		const heights = lines.map(
			(line) =>
				ctx.measureText(line).actualBoundingBoxAscent +
				ctx.measureText(line).actualBoundingBoxDescent
		);

		const width = Math.max(...widths);
		const height = heights.reduce((a, b) => a + b, 0);

		return { width, height };
	}

	function placeLabel(ctx: CanvasRenderingContext2D, loc: Loc, text: string) {
		let fontSize = 21;
		let width = 0;
		let height = 0;
		let [x, y] = loc;
		const lines = text.split('\n');
		const numLines = lines.length;

		do {
			fontSize -= 1;
			const sizes = size(ctx, text, fontSize);
			width = sizes.width || 0;
			height = sizes.height || 0;
		} while (width > 178 || height > 62);

		ctx.font = `${fontSize}px Arial`;

		const yPadding = (62 - height) / (numLines + 1);

		for (const line of lines) {
			const metrics = ctx.measureText(line);
			const textWidth = metrics.width;
			const textHeight = metrics.actualBoundingBoxAscent + metrics.actualBoundingBoxDescent;

			const xPadding = (178 - textWidth) / 2;
			y += yPadding + textHeight;

			ctx.fillText(line, x + xPadding, y);
		}
	}

	const locs = [
		[89, 118],
		[89, 220],
		[89, 301],
		[89, 403],
		[89, 517],
		[89, 620],
		[89, 700],
		[89, 805],
		[1613, 118],
		[1613, 220],
		[1613, 301],
		[1613, 403],
		[1613, 517],
		[1613, 620],
		[1613, 710],
		[1613, 805]
	];

	const labels = ['1', '8', '3', '6', '4', '5', '2', '7', '1', '8', '3', '6', '4', '5', '2', '7'];

	const draw = () => {
		const ctx = canvas.getContext('2d');
		const image = new Image();
		image.src = src;

		image.onload = () => {
			ctx.drawImage(image, 0, 0);

			locs.forEach((loc, i) => {
				const text = texts[i];
				placeLabel(ctx, loc, text);
			});

			/*
			ctx.beginPath();
			ctx.strokeStyle = 'red';
			ctx.rect(89, 118, 178, 62); // 1
			ctx.rect(89, 220, 178, 62); // 8
			ctx.rect(89, 301, 178, 62); // 3
			ctx.rect(89, 403, 178, 62); // 6
			ctx.rect(89, 517, 178, 62); // 4
			ctx.rect(89, 620, 178, 62); // 5
			ctx.rect(89, 700, 178, 62); // 2
			ctx.rect(89, 805, 178, 62); // 7

			ctx.rect(1613, 118, 178, 62); // 1
			ctx.rect(1613, 220, 178, 62); // 8
			ctx.rect(1613, 301, 178, 62); // 3
			ctx.rect(1613, 403, 178, 62); // 6
			ctx.rect(1613, 517, 178, 62); // 4
			ctx.rect(1613, 620, 178, 62); // 5
			ctx.rect(1613, 710, 178, 62); // 2
			ctx.rect(1613, 805, 178, 62); // 7

			ctx.stroke();
            */
		};
	};

	onMount(draw);

	const change = (i) => (evt) => {
		texts[i] = evt.target.value;
		draw();
	};

	function downloadImage() {
		const canvasUrl = canvas.toDataURL();
		// Create an anchor, and set the href value to our data URL
		const createEl = document.createElement('a');
		createEl.href = canvasUrl;

		// This is the name of our downloaded file
		createEl.download = 'download-this-canvas';

		// Click the download button, causing a download, and then remove it
		createEl.click();
		createEl.remove();
	}
</script>

<canvas bind:this={canvas} {width} {height} />

<div>
	<button on:click={downloadImage} class="border-2 p-2">download</button>
</div>

{#each texts as text, i}
	<div class="flex flex-col p-2">
		<label for={i}>{labels[i]}</label>
		<textarea class="border-2" value={text} on:keyup={change(i)} />
	</div>
{/each}
