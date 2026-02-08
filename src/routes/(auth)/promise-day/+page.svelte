<script>
	import { base } from '$app/paths';
</script>

<svelte:head>
	<title>Promise Day — Valentine Week</title>
</svelte:head>

<main class="game">
	<h1>🤝 Promise Day</h1>
	<p class="promise-text">I promise to be with you forever</p>
	<a href={base + '/days'} class="back">← Return to days</a>
</main>

<style>
	.game {
		min-height: 100vh;
		padding: 2rem 1rem;
		background: linear-gradient(180deg, #f0fff0 0%, #e8f5e9 100%);
		text-align: center;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		gap: 2rem;
	}
	h1 { color: #2d5a27; margin-bottom: 0; }
	.promise-text {
		font-size: 1.5rem;
		color: #333;
		max-width: 360px;
		line-height: 1.6;
		margin: 0;
	}
	.back {
		padding: 0.6rem 1.25rem;
		background: #2d5a27;
		color: white;
		border-radius: 10px;
		text-decoration: none;
		font-weight: 600;
		min-height: 44px;
		display: inline-flex;
		align-items: center;
		justify-content: center;
	}
	.back:hover { background: #1e3d1a; }
</style>
