<script lang="ts">
	interface Props {
		src: string;
		title: string;
	}
	let { src, title }: Props = $props();
	let time = $state(0);
	let duration = $state(0);
	let paused = $state(true);
	function format(time: number) {
		if (isNaN(time)) return '...';
		const minutes = Math.floor(time / 60);
		const seconds = Math.floor(time % 60);
		return `${minutes}:${seconds < 10 ? `0${seconds}` : seconds}`;
	}
</script>

<div class="font-mono text-yellow-c">
	<audio onended={() => (time = 0)} bind:duration bind:currentTime={time} bind:paused {src}></audio>
	<button
		class="flex w-full flex-auto justify-between text-xl hover:underline"
		onclick={() => (paused = !paused)}
		><h3>{title}</h3>
		{#if time === 0}
			<p>{format(duration)}</p>
		{:else}
			<p>{format(time)}</p>
		{/if}
	</button>
</div>
