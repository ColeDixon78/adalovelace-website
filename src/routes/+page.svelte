<script>
	import sample from '$lib/assets/sample.wav';
	import AudioPlayer from '$lib/components/audio-player.svelte';
	let height = $state(0);
	let width = $state(0);
	const rows = 6;
	const columns = 9;
	const margin = {
		top: 10,
		bottom: 10,
		right: 10,
		left: 10
	};
	const centerX = $derived(width / (columns * 2));
	const centerY = $derived(height / (rows * 2));
</script>

<main bind:clientHeight={height} bind:clientWidth={width} class="h-screen w-screen bg-soft-pink">
	<h1 class="font-mono text-4xl font-semibold text-white">Ada Lovelace</h1>

	<!-- <svg {width} {height}> -->
	<!-- 	<defs> -->
	<!-- 		<pattern id="Pattern" x="0" y="0" width={1 / columns} height={1 / rows}> -->
	<!-- 			<circle cx={centerX} cy={centerY} r="20" fill="white" /> -->
	<!-- 		</pattern> -->
	<!-- 	</defs> -->
	<!-- 	<rect fill="url(#Pattern)" {width} {height} /> -->
	<!-- </svg> -->
	<!---->
	<AudioPlayer title="test" src={sample} />
</main>
