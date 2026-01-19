<script lang="ts">
	import sample from '$lib/assets/sample.wav';
	import AudioPlayer from '$lib/components/audio-player.svelte';
	let height = $state(0);
	let width = $state(0);
	$inspect(height);
	const rows = 5;
	const columns = 10;
	const xBandwidth = $derived(width / columns);
	const yBandwidth = $derived(height / rows);
	const radius = $derived(Math.min(Math.min(xBandwidth / 3, yBandwidth / 3), 24));
	// const colorPalette = ['9e0031', '8e0045', '770058', '600047', '44001a'];
	const colorPalette = ['650d1b', '823200', '9b3d12', 'ae8e1c', 'c1df1f'];
	// const colorPalette = ['e3b505', '95190c', '610345', '107e7d', '044b7f'];
	const colorArray: string[][] = $state(Array(rows).fill(Array(columns).fill('white')));
	function randomizeColors() {
		for (let row = 0; row < rows; row++) {
			for (let col = 0; col < columns; col++) {
				if (Math.random() < 0) {
					colorArray[row][col] = 'white';
					continue;
				}
				const randomElement = colorPalette[Math.floor(Math.random() * colorPalette.length)];
				colorArray[row][col] = '#' + randomElement;
			}
		}
	}

	$effect(() => randomizeColors());
</script>

<main class="flex h-screen w-screen flex-col bg-onyx">
	<h1 class="mx-8 mt-4 font-mono text-4xl font-semibold text-[#c1df1f]">Ada Lovelace</h1>

	<svg class="flex-auto" bind:clientHeight={height} bind:clientWidth={width}>
		{#each { length: rows }, row}
			{#each { length: columns }, col}
				{@const color = colorArray[row][col]}
				{@const staggerX = (Math.random() * xBandwidth) / 20}
				{@const staggerY = (Math.random() * yBandwidth) / 20}
				<svg
					x={xBandwidth * col + staggerX}
					y={yBandwidth * row + staggerY}
					width={xBandwidth}
					height={yBandwidth}
				>
					<circle class="light" cx={xBandwidth / 2} cy={yBandwidth / 2} r={radius} fill={color} />
				</svg>
			{/each}
		{/each}
	</svg>

	<AudioPlayer title="test" src={sample} />
</main>

<style>
	.light {
		filter: blur(0.18em) drop-shadow(0px 0px 8px white);
	}
</style>
