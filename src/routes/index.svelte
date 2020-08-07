<script>
	import { stores } from '@sapper/app';
	import { onMount } from 'svelte';

	const { page } = stores();

	const meta = {
		name: 'Pacpic',
		desc: 'Fotos de bultos para organizacion',
		url: $page.host + $page.path,
	};

	let items = [];

	onMount(() => {
		let storage = window.localStorage;

		for (let i in storage) {
			if (typeof storage[i] === 'string' && storage[i].startsWith('{')) {
				items = [...items, JSON.parse(storage[i])];
			}
		}
	});
</script>

<style lang="scss">
	@import 'verdu/vars';

	@media (max-width: $mobile) {
		.home {
			padding: 20px;
		}
	}

	.card {
		position: relative;
		background: $white;

		.created {
			position: absolute;
			top: 10px;
			right: 15px;
			font-size: 12px;
			font-family: 'Operator Mono Lig';
			color: $grey;
		}

		p {
			font-size: 14px;
		}
	}
</style>

<svelte:head>
	<link rel="canonical" href={meta.url} />
	<meta name="description" content={meta.desc} />
	<title>{meta.name}</title>
	<!-- Social -->
	<meta property="og:site_name" content={meta.name} />
	<meta property="og:title" content={meta.name} />
	<meta property="og:url" content={meta.url} />
	<meta property="og:image" content="{meta.url}/mobile.png" />
</svelte:head>

<div class="home col acenter w100 p40">
	{#each items as { _created, to, code }}
		<div class="card box round w100 p20 mb2">
			<div class="created">{_created}</div>
			<h3 bind:textContent={to} contenteditable />
			<p>{code}</p>
		</div>
	{/each}
</div>
