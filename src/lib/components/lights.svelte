<script lang="ts">
	import { onMount } from 'svelte';
	let height = $state(0);
	let width = $state(0);
	let mouse = $state({ x: 0, y: 0 });
	let { colorChoice } = $props();
	const rows = 6;
	const columns = 6;
	const lightCount = rows * columns;
	const xBandwidth = $derived(width / columns);
	const yBandwidth = $derived(height / rows);
	const radius = $derived(Math.min(Math.min(xBandwidth / 3, yBandwidth / 3), 40));
	const colorPalettes = [
		['9e0031', '8e0045', '770058', '600047', '44001a'],
		['af3bbf', 'a14ebf', '6c91bf', '5fb0b7', '5bc8af'],
		['91f9e5', '76f7bf', '5fdd9d', '499167', '3f4531'],
		['db995a', '654236', 'd6d4a0', 'da7635', 'e24e1b'],
		['f4f1de', 'e07a5f', '3d405b', '81b29a', 'f2cc8f']
	];
	// const colorPalette = ['650d1b', '823200', '9b3d12', 'ae8e1c', 'c1df1f'];
	// const colorPalette = ['e3b505', '95190c', '610345', '107e7d', '044b7f'];
	const colorArray: string[] = $state(Array(lightCount).fill('white'));
	function randomizeColors() {
		const colorClasses = ['var1', 'var2', 'var3', 'var4', 'var5'];
		for (let index = 0; index < lightCount; index++) {
			const randomElement = colorClasses[Math.floor(Math.random() * colorClasses.length)];
			colorArray[index] = randomElement;
		}
	}
	let ref: HTMLElement | undefined = $state();
	function updateColorPalette(colors: string[]) {
		//update css variables here
		if (!ref) {
			return;
		}
		for (let i = 0; i < colors.length; i++) {
			ref.style.setProperty(`--light-color-${i + 1}`, '#' + colors[i]);
		}
	}

	$effect(() => {
		updateColorPalette(colorPalettes[colorChoice % colorPalettes.length]);
		randomizeColors();
	});
	onMount(() => setInterval(randomizeColors, 2000));
</script>

<div
	bind:this={ref}
	class="relative grid flex-auto grid-cols-6"
	bind:clientHeight={height}
	bind:clientWidth={width}
	onpointermove={(e) => {
		mouse.x = e.clientX;
		mouse.y = e.clientY;
	}}
>
	{#each { length: lightCount }, index}
		{@const staggerX = (Math.random() * xBandwidth) / 20}
		{@const staggerY = (Math.random() * yBandwidth) / 20}
		{@const distance = Math.sqrt(
			(mouse.x - (width * (index % columns)) / columns) ** 2 +
				(mouse.y - (height * Math.floor(index % rows)) / rows) ** 2
		)}
		{@const staggerR = radius * (distance / width)}
		{@const color = colorArray[index]}
		<div class="relative col-span-1">
			<div
				class="light {color}"
				style="transform: translate({staggerX}px, {staggerY}px); width: {radius +
					staggerR}px; height: {radius + staggerR}px;"
			></div>
		</div>
	{/each}
</div>

<style>
	.light {
		opacity: 0.8;
		position: absolute;
		top: calc(50% - 20px);
		left: calc(50% - 20px);
		border-radius: 50%;
		background-color: #fff;
		transition:
			background-color 0.4s,
			width 0.2s,
			height 0.2s,
			box-shadow 0.4s;
	}

	.var1 {
		--primary: var(--light-color-1);
		border-style: solid;
		border-color: var(--primary);
		background-color: var(--primary);
		/* background: radial-gradient(color-mix(in hsl, var(--primary), #fff 15%), var(--primary)); */
		box-shadow:
			inset 0 0 5px color-mix(in hsl, var(--primary), #fff 30%),
			0 0 10px 5px var(--primary);
	}

	.var2 {
		--primary: var(--light-color-2);
		border-style: solid;
		border-color: var(--primary);
		background-color: var(--primary);
		/* background: radial-gradient(color-mix(in hsl, var(--primary), #fff 15%), var(--primary)); */
		box-shadow:
			inset 0 0 5px color-mix(in hsl, var(--primary), #fff 30%),
			0 0 10px 5px var(--primary);
	}

	.var3 {
		--primary: var(--light-color-3);
		border-style: solid;
		border-color: var(--primary);
		background-color: var(--primary);
		/* background: radial-gradient(color-mix(in hsl, var(--primary), #fff 15%), var(--primary)); */
		box-shadow:
			inset 0 0 5px color-mix(in hsl, var(--primary), #fff 30%),
			0 0 10px 5px var(--primary);
	}

	.var4 {
		--primary: var(--light-color-4);
		border-style: solid;
		border-color: var(--primary);
		background-color: var(--primary);
		/* background: radial-gradient(color-mix(in hsl, var(--primary), #fff 15%), var(--primary)); */
		box-shadow:
			inset 0 0 5px color-mix(in hsl, var(--primary), #fff 30%),
			0 0 10px 5px var(--primary);
	}

	.var5 {
		--primary: var(--light-color-5);
		border-style: solid;
		border-color: var(--primary);
		background-color: var(--primary);
		/* background: radial-gradient(color-mix(in hsl, var(--primary), #fff 15%), var(--primary)); */
		box-shadow:
			inset 0 0 5px color-mix(in hsl, var(--primary), #fff 30%),
			0 0 10px 5px var(--primary);
	}
</style>
