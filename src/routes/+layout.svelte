<script>
	import favicon from '$lib/assets/favicon.svg';

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
	<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover" />
</svelte:head>

{@render children()}

<style>
	:global(html) {
		-webkit-text-size-adjust: 100%;
	}
	:global(body) {
		padding: env(safe-area-inset-top) env(safe-area-inset-right) env(safe-area-inset-bottom) env(safe-area-inset-left);
	}
	@media (max-width: 480px) {
		:global(main) {
			padding-left: 0.75rem;
			padding-right: 0.75rem;
		}
		:global(h1) {
			font-size: clamp(1.35rem, 5vw, 2rem);
		}
	}
</style>
