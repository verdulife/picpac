<script>
	import { onMount } from 'svelte';
	import { fly } from 'svelte/transition';

	export let move = 600;
	export let duration = 300;

	let show = false;
	let enter = {
		delay: duration,
		x: +move,
		y: 0,
		duration: duration,
	};

	export let leave = {
		x: -move,
		y: 0,
		duration: duration,
	};

	onMount(() => {
		show = true;
	});
</script>

<style>
	div {
		position: relative;
	}
</style>

{#if show}
	<div in:fly={enter} out:fly={leave}>
		<slot />
	</div>
{/if}
