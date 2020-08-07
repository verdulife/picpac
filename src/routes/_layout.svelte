<script>
	import Nav from '../components/Nav.svelte';

	export let segment;
</script>

<style lang="scss" global>
	@import 'verdu/verdu';
</style>

<div id="app" class="layout view col">
	<div class="scroll">
		<main class="view">
			<slot />
		</main>
	</div>

	<Nav {segment} />
</div>
