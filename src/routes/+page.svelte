<script lang="ts">
	import sample from '$lib/assets/sample.wav';
	import AudioPlayer from '$lib/components/audio-player.svelte';
	let height = $state(0);
	let width = $state(0);
	let mouse = $state({ x: 0, y: 0 });
	const rows = 6;
	const columns = 10;
	const lightCount = rows * columns;
	const xBandwidth = $derived(width / columns);
	const yBandwidth = $derived(height / rows);
	const radius = $derived(Math.min(Math.min(xBandwidth / 3, yBandwidth / 3), 40));
	const colorPalette = ['9e0031', '8e0045', '770058', '600047', '44001a'];
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
		updateColorPalette(colorPalette);
		randomizeColors();
		setInterval(randomizeColors, 2000);
	});
</script>

<main
	bind:this={ref}
	onpointermove={(e) => {
		mouse.x = e.clientX;
		mouse.y = e.clientY;
	}}
	class="flex h-screen w-screen flex-col bg-onyx"
>
	<header class="border border-b-[#c1df1f] p-2">
		<h1 class="font-mono text-4xl font-semibold text-[#c1df1f]">Ada Lovelace</h1>
	</header>

	<div
		class="relative grid flex-auto grid-cols-10"
		bind:clientHeight={height}
		bind:clientWidth={width}
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

	<AudioPlayer title="test" src={sample} />
</main>

<style>
	.light {
		opacity: 0.8;
		position: absolute;
		top: calc(50% - 20px);
		left: calc(50% - 20px);
		border-radius: 50%;
		background-color: #fff;
		transition:
			background-color 0.2s,
			width 0.2s,
			height 0.2s;
	}

	.var1 {
		--my-color: var(--light-color-1);
		border-style: solid;
		border-color: var(--my-color);
		background-color: var(--my-color);
		box-shadow:
			inset 0 0 5px color-mix(in hsl, var(--my-color), #fff 30%),
			0 0 10px 5px var(--my-color);
	}

	.var2 {
		--my-color: var(--light-color-2);
		border-style: solid;
		border-color: var(--my-color);
		background-color: var(--my-color);
		box-shadow:
			inset 0 0 5px color-mix(in hsl, var(--my-color), #fff 30%),
			0 0 10px 5px var(--my-color);
	}

	.var3 {
		--my-color: var(--light-color-3);
		border-style: solid;
		border-color: var(--my-color);
		background-color: var(--my-color);
		box-shadow:
			inset 0 0 5px color-mix(in hsl, var(--my-color), #fff 30%),
			0 0 10px 5px var(--my-color);
	}

	.var4 {
		--my-color: var(--light-color-4);
		border-style: solid;
		border-color: var(--my-color);
		background-color: var(--my-color);
		box-shadow:
			inset 0 0 5px color-mix(in hsl, var(--my-color), #fff 30%),
			0 0 10px 5px var(--my-color);
	}

	.var5 {
		--my-color: var(--light-color-5);
		border-style: solid;
		border-color: var(--my-color);
		background-color: var(--my-color);
		box-shadow:
			inset 0 0 5px color-mix(in hsl, var(--my-color), #fff 30%),
			0 0 10px 5px var(--my-color);
	}
</style>
