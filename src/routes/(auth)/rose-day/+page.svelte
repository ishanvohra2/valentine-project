<script>
	import { onMount, onDestroy } from 'svelte';

	let score = 0;
	let misses = 0;
	let gameOver = false;
	let highScore = 0;
	let roses = [];
	let id = 0;
	let spawnInterval;
	let fallInterval;
	const ROSE_WIDTH = 44;
	const BOTTOM_Y = 320;
	const CATCH_THRESHOLD = 30;

	function spawnRose() {
		if (gameOver) return;
		roses = [
			...roses,
			{
				id: id++,
				x: Math.random() * (100 - 12) + 6,
				y: 0,
				caught: false
			}
		];
	}

	function catchRose(e, rose) {
		if (rose.caught || gameOver) return;
		rose.caught = true;
		score += 1;
		roses = roses.filter((r) => r.id !== rose.id);
		if (score > highScore) highScore = score;
	}

	function tick() {
		if (gameOver) return;
		let updated = false;
		roses = roses.map((r) => {
			if (r.caught) return r;
			const newY = r.y + 2;
			if (newY >= BOTTOM_Y - 20) {
				misses += 1;
				updated = true;
				return null;
			}
			return { ...r, y: newY };
		}).filter(Boolean);
		if (misses >= 5) {
			gameOver = true;
			if (score > highScore) highScore = score;
		}
	}

	onMount(() => {
		spawnInterval = setInterval(spawnRose, 1200);
		fallInterval = setInterval(tick, 50);
	});
	onDestroy(() => {
		clearInterval(spawnInterval);
		clearInterval(fallInterval);
	});

	function playAgain() {
		score = 0;
		misses = 0;
		gameOver = false;
		roses = [];
	}
</script>

<svelte:head>
	<title>Rose Day — Valentine Week</title>
</svelte:head>

<main class="game">
	<h1>Catch the roses for you</h1>
	<p class="instruction">Click each rose before it hits the ground. 5 misses = game over!</p>
	<div class="hud">
		<span>Caught: {score}</span>
		<span>Misses: {misses}/5</span>
		<span>High score: {highScore}</span>
	</div>
	<div class="zone" class:over={gameOver}>
		{#each roses as rose (rose.id)}
			<button
				class="rose"
				style="left: {rose.x}%; top: {rose.y}px;"
				onclick={(e) => catchRose(e, rose)}
				disabled={gameOver}
			>
				🌹
			</button>
		{/each}
		{#if gameOver}
			<div class="over">
				<p>Game over!</p>
				<p>Final score: {score} | Best: {highScore}</p>
				<button type="button" class="again" onclick={playAgain}>Play again</button>
			</div>
		{/if}
	</div>
	<a href="/days" class="back">← Back to days</a>
</main>

<style>
	.game {
		min-height: 100vh;
		padding: 1.5rem 1rem;
		background: linear-gradient(180deg, #fff0f5 0%, #ffd1dc 100%);
		text-align: center;
	}
	h1 { color: #8b0045; margin-bottom: 0.25rem; font-size: 1.75rem; }
	.instruction { color: #5c0032; margin-bottom: 0.75rem; font-size: 0.95rem; }
	.hud {
		display: flex;
		justify-content: center;
		gap: 1.5rem;
		margin-bottom: 1rem;
		font-weight: 600;
		color: #5c0032;
	}
	.zone {
		position: relative;
		height: 340px;
		max-width: 400px;
		margin: 0 auto 1rem;
		background: linear-gradient(180deg, rgba(255,255,255,0.6) 0%, rgba(255,182,193,0.4) 100%);
		border-radius: 12px;
		border: 2px solid #ffb6c1;
	}
	.zone.over { overflow: hidden; }
	.rose {
		position: absolute;
		transform: translateX(-50%);
		font-size: 2.5rem;
		background: none;
		border: none;
		cursor: pointer;
		padding: 0;
		line-height: 1;
		transition: transform 0.1s;
	}
	.rose:hover:not(:disabled) { transform: translateX(-50%) scale(1.15); }
	.over {
		position: absolute;
		inset: 0;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		background: rgba(255, 255, 255, 0.95);
		border-radius: 10px;
		z-index: 10;
		pointer-events: auto;
		box-shadow: inset 0 0 0 2px rgba(139, 0, 69, 0.2);
	}
	.over p { margin: 0.25rem 0; color: #8b0045; font-weight: 600; }
	.again { margin-top: 0.5rem; padding: 0.5rem 1rem; background: #ff69b4; color: white; border: none; border-radius: 8px; font-weight: 600; cursor: pointer; font-size: 1rem; }
	.back { color: #8b0045; text-decoration: none; font-weight: 600; }
	.back:hover { text-decoration: underline; }
</style>
