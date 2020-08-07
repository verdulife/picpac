<script>
	import { stores, goto } from '@sapper/app';
	import { onMount } from 'svelte';
	import Quagga from 'quagga';

	const { page } = stores();

	const meta = {
		name: 'Capture 📷',
		desc: 'Captura los Barcodes de los paquetes',
		url: $page.host + $page.path,
	};

	let canvas;
	let barcode;
	let active = false;
	let items = [];

	onMount(() => {
		let storage = window.localStorage;

		for (let i in storage) {
			if (typeof storage[i] === 'string' && storage[i].startsWith('{')) {
				items = [...items, JSON.parse(storage[i])];
			}
		}

		Quagga.init(
			{
				inputStream: {
					name: 'Live',
					type: 'LiveStream',
					target: canvas,
					constraints: {
						width: window.innerWidth,
						height: window.innerHeight,
					},
				},
				decoder: {
					readers: ['code_128_reader', 'ean_reader', 'ean_8_reader', 'code_39_reader', 'code_39_vin_reader', 'codabar_reader', 'upc_reader', 'upc_e_reader', 'i2of5_reader', '2of5_reader', 'code_93_reader'],
				},
			},
			function (err) {
				if (err) {
					console.log(err);
					return;
				}
				Quagga.start();
				Quagga.onDetected((data) => {
					if (data) {
						if (data.angle < 0.5 && data.angle > -0.5) {
							barcode = data.codeResult.code;
							active = true;

							for (let i = 0; i < items.length; i++) {
								let item = items[i];

								if (item.code === barcode) {
									alert(item.to);
								}
							}
						}
					}
				});
			}
		);
	});

	function saveCode() {
		storage.setItem(
			item.code,
			JSON.stringify({
				_created: new Date().toLocaleDateString(),
				code: barcode,
				to: 'Sin nombre',
			})
		);
		goto('/');
	}

	function cancelCode() {
		active = false;
	}
</script>

<style lang="scss">
	@import 'verdu/vars';

	.modal {
		position: fixed;
		top: 50%;
		left: 0;
		right: 0;
		transform: translateY(-50%);
		margin: 0 auto;
		width: calc(100% - 40px);
		background: $white;
		bottom: 1px solid $border;
		border-radius: 20px;
		padding: 30px;
		opacity: 0;
		pointer-events: none;
		transition: 200ms;

		h3 {
			margin-bottom: 20px;
		}

		p {
			width: 100%;
			background: $border;
			font-family: 'Operator Mono Lig';
			font-size: 24px;
			text-align: center;
			border-radius: 10px;
			padding: 10px 20px;
			margin-bottom: 40px;
		}

		button {
			width: 250px;
			justify-content: center;
			margin-bottom: 10px;

			&:first-of-type {
				color: $white;
			}
		}
	}

	.active {
		opacity: 1;
		pointer-events: all;
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

<div class="capture view" bind:this={canvas} />
<div class="modal col fcenter" class:active>
	<h3>Codigo registrado</h3>
	<p>{barcode}</p>
	<div class="col acenter w100">
		<button class="pri semi" on:click={saveCode}>AÑADIR</button>
		<button class="outpri semi" on:click={cancelCode}>CANCELAR</button>
	</div>
</div>
