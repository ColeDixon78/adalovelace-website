<script lang="ts">
	import sample from '$lib/assets/sample.wav';
	import AudioPlayer from '$lib/components/audio-player.svelte';
	let height = $state(0);
	let width = $state(0);
	const rows = 5;
	const columns = 10;
	const lightCount = rows * columns;
	const xBandwidth = $derived(width / columns);
	const yBandwidth = $derived(height / rows);
	const radius = $derived(Math.min(Math.min(xBandwidth / 3, yBandwidth / 3), 20));
	// const colorPalette = ['9e0031', '8e0045', '770058', '600047', '44001a'];
	// const colorPalette = ['650d1b', '823200', '9b3d12', 'ae8e1c', 'c1df1f'];
	// const colorPalette = ['e3b505', '95190c', '610345', '107e7d', '044b7f'];
	const colorPalette = ['var1', 'var2', 'var3', 'var4', 'var5'];
	const colorArray: string[] = $state(Array(lightCount).fill('white'));
	function randomizeColors() {
		for (let index = 0; index < rows; index++) {
			const randomElement = colorPalette[Math.floor(Math.random() * colorPalette.length)];
			colorArray[index] = '#' + randomElement;
		}
	}
	function updateColorPalette() {
		//update css variables here
	}

	$effect(() => randomizeColors());
</script>

<main class="flex h-screen w-screen flex-col bg-onyx">
	<h1 class="mx-8 mt-4 font-mono text-4xl font-semibold text-[#c1df1f]">Ada Lovelace</h1>

	<div
		class="relative grid flex-auto grid-cols-10"
		bind:clientHeight={height}
		bind:clientWidth={width}
	>
		{#each { length: lightCount }}
			{@const staggerX = (Math.random() * xBandwidth) / 20}
			{@const staggerY = (Math.random() * yBandwidth) / 20}
			{@const staggerR = Math.random() * (radius / 4)}
			{@const color = colorPalette[Math.floor(Math.random() * colorPalette.length)]}
			<div class="relative col-span-1">
				<div class="light {color}"></div>
			</div>
		{/each}
	</div>

	<AudioPlayer title="test" src={sample} />
</main>

<style>
	main {
		--light-color-1: #650d1b;
		--light-color-2: #823200;
		--light-color-3: #9b3d12;
		--light-color-4: #ae8e1c;
		--light-color-5: #c1df1f;
	}
	.light {
		position: absolute;
		top: calc(50% - 20px);
		left: calc(50% - 20px);
		width: 40px;
		height: 40px;
		border-radius: 50%;
	}

	.var1 {
		background-color: var(--light-color-1);
		box-shadow:
			0 0 10px 0px #fff,
			0 0 20px 0px var(--light-color-1);
	}
	.var2 {
		background-color: var(--light-color-2);
		box-shadow:
			0 0 10px 0px #fff,
			0 0 20px 0px var(--light-color-2);
	}
	.var3 {
		background-color: var(--light-color-3);
		box-shadow:
			0 0 10px 0px #fff,
			0 0 20px 0px var(--light-color-3);
	}
	.var4 {
		background-color: var(--light-color-4);
		box-shadow:
			0 0 10px 0px #fff,
			0 0 20px 0px var(--light-color-4);
	}
	.var5 {
		background-color: var(--light-color-5);
		box-shadow:
			0 0 10px 0px #fff,
			0 0 20px 0px var(--light-color-5);
	}
</style>
