<script>
	export let segment;

	const routes = [
		{ href: '.', name: 'LISTA' },
		{ href: 'capture', name: 'CAMARA' },
	];
</script>

<style lang="scss">
	@import 'verdu/vars';

	nav {
		position: fixed;
		bottom: 20px;
		left: 20px;
		right: 20px;
		min-height: 60px;

		li:first-of-type {
			border-right: 1px solid $border;
		}

		a {
			transition: 200ms;
		}

		.active {
			a {
				color: $pri;
			}
		}
	}
</style>

<nav class="box white round row fcenter">
	<ul class="row w100 h100 jaround">
		{#each routes as { href, name }}
			<li class="row fcenter w50 h100" class:active={segment === (href === '.' ? undefined : href) ? 'page' : undefined}>
				<a rel="prefetch" class="btn w100 fcenter" {href}>{name}</a>
			</li>
		{/each}
	</ul>
</nav>
