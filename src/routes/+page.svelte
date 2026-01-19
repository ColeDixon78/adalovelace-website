<script lang="ts">
	import sample from '$lib/assets/sample.wav';
	import AudioPlayer from '$lib/components/audio-player.svelte';
	import Lights from '$lib/components/lights.svelte';
	let colorChoice = $state(0);
	function nextColor() {
		colorChoice++;
	}
</script>

<svelte:window
	onkeydown={(e) => {
		if (e.key === ' ') {
			nextColor();
		}
	}}
/>

<main class="flex h-screen w-screen flex-row bg-onyx">
	<div class="absolute top-8 left-16 z-50 backdrop-blur-sm">
		<h1 class="font-mono text-4xl font-semibold text-yellow-c">Ada Lovelace</h1>
		<div class="flex flex-col gap-4">
			<AudioPlayer title="Falls On You" src={sample} />
			<AudioPlayer title="Through the TV" src={sample} />
			<AudioPlayer title="Don't Say Please" src={sample} />
			<AudioPlayer title="Quiet Now" src={sample} />
			<AudioPlayer title="Carport Light" src={sample} />
		</div>
	</div>
	<div class="flex flex-auto">
		<Lights {colorChoice} />
	</div>
</main>
