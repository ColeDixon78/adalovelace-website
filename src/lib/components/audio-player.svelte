<script lang="ts">
	interface Props {
		src: string;
		title: string;
	}
	let { src, title }: Props = $props();
	let time = $state(0);
	let duration = $state(0);
	let paused = $state(true);
</script>

<div>
	<h2>{title}</h2>
	<audio bind:duration bind:currentTime={time} bind:paused {src}></audio>
	<button onclick={() => (paused = !paused)}> Play</button>
</div>
