<script>
	import { onMount, onDestroy } from 'svelte';

	let clawX = 50;
	let clawDir = 1;
	const CLAW_MOVE_SPEED = 0.2;
	const CLAW_MIN_X = 18;
	const CLAW_MAX_X = 82;

	let phase = 'idle'; // idle | dropping | rising | won | lost
	let clawY = 0;
	const TEDDY_X = 50;
	const CLAW_DROP_SPEED = 2;
	const CLAW_RISE_SPEED = 1.5;
	const HIT_MARGIN = 18;
	const DROP_TARGET = 140;
	let animId;
	let didCaptureTeddy = false;

	function drop() {
		if (phase !== 'idle') return;
		phase = 'dropping';
		clawY = 0;
		didCaptureTeddy = false;
	}

	function loop() {
		if (phase === 'idle') {
			clawX += CLAW_MOVE_SPEED * clawDir;
			if (clawX >= CLAW_MAX_X) clawDir = -1;
			if (clawX <= CLAW_MIN_X) clawDir = 1;
		} else if (phase === 'dropping') {
			clawY += CLAW_DROP_SPEED;
			if (clawY >= DROP_TARGET) {
				const hit = Math.abs(clawX - TEDDY_X) < HIT_MARGIN;
				if (hit) {
					didCaptureTeddy = true;
					phase = 'rising';
				} else {
					phase = 'lost';
				}
			}
		} else if (phase === 'rising') {
			clawY -= CLAW_RISE_SPEED;
			if (clawY <= -10) {
				phase = 'won';
			}
		}
		animId = requestAnimationFrame(loop);
	}

	onMount(() => {
		animId = requestAnimationFrame(loop);
	});
	onDestroy(() => {
		if (animId) cancelAnimationFrame(animId);
	});
</script>

<svelte:head>
	<title>Teddy Day — Valentine Week</title>
</svelte:head>

<main class="game">
	<h1>🧸 Teddy Day</h1>
	<p class="instruction">Time the drop when the claw is over the teddy!</p>
	{#if phase === 'won' && didCaptureTeddy}
		<p class="result win">You got the teddy! 🧸💕</p>
	{:else if phase === 'lost'}
		<p class="result lose">Missed! <a href="/teddy-day">Try again</a></p>
	{:else if phase === 'idle'}
		<div class="controls">
			<button type="button" class="drop" onclick={drop}>Drop claw</button>
		</div>
	{/if}
	<div class="machine">
		<div class="claw" style="left: {clawX}%; top: {clawY}px;">
			<img src="/claw.svg" alt="Claw" class="claw-img" />
		</div>
		<div class="prizes">
			<div class="teddy" style="left: {TEDDY_X}%;">🧸</div>
			<div class="extra prize1">🐻</div>
			<div class="extra prize2">❤️</div>
			<div class="extra prize3">🐰</div>
			<div class="extra prize4">🎀</div>
			<div class="extra prize5">💝</div>
			<div class="extra prize6">🌸</div>
			<div class="extra prize7">🦊</div>
			<div class="extra prize8">🐱</div>
			<div class="extra prize9">⭐</div>
			<div class="extra prize10">🎁</div>
		</div>
		<div class="machine-glass"></div>
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
	h1 { color: #8b0045; margin-bottom: 0.5rem; }
	.instruction { color: #5c0032; margin-bottom: 1rem; }
	.controls {
		display: flex;
		justify-content: center;
		gap: 0.5rem;
		margin-bottom: 1rem;
		flex-wrap: wrap;
	}
	.controls button {
		padding: 0.5rem 1.5rem;
		min-height: 44px;
		background: #8b0045;
		color: white;
		border: none;
		border-radius: 8px;
		cursor: pointer;
		font-weight: 600;
	}
	.result { font-weight: 600; margin-bottom: 1rem; }
	.result.win { color: #2d7d2d; }
	.result.lose { color: #8b0045; }
	.result a { color: #ff69b4; }
	.machine {
		position: relative;
		height: 240px;
		max-width: 360px;
		margin: 0 auto 1.5rem;
		background: linear-gradient(180deg, #5a5a5a 0%, #3d3d3d 50%, #2d2d2d 100%);
		border-radius: 16px;
		border: 6px solid #333;
		box-shadow: inset 0 0 30px rgba(0,0,0,0.4), 0 6px 20px rgba(0,0,0,0.3);
	}
	.machine-glass {
		position: absolute;
		inset: 4px;
		border-radius: 10px;
		background: linear-gradient(180deg, rgba(100,150,255,0.08) 0%, transparent 50%);
		pointer-events: none;
	}
	.claw {
		position: absolute;
		transform: translateX(-50%);
		z-index: 5;
	}
	.claw-img {
		display: block;
		width: 48px;
		height: 60px;
		filter: drop-shadow(0 2px 4px rgba(0,0,0,0.4));
	}
	.prizes {
		position: absolute;
		inset: 0;
		pointer-events: none;
	}
	.teddy {
		position: absolute;
		bottom: 24px;
		transform: translateX(-50%);
		font-size: 2.75rem;
		z-index: 1;
		filter: drop-shadow(0 2px 4px rgba(0,0,0,0.3));
	}
	.extra {
		position: absolute;
		font-size: 1.35rem;
		z-index: 1;
		opacity: 0.92;
	}
	.prize1 { left: 8%; bottom: 22px; }
	.prize2 { left: 22%; bottom: 28px; }
	.prize3 { left: 36%; bottom: 20px; }
	.prize4 { left: 64%; bottom: 22px; }
	.prize5 { left: 78%; bottom: 26px; }
	.prize6 { right: 8%; bottom: 24px; }
	.prize7 { left: 14%; bottom: 38px; }
	.prize8 { left: 72%; bottom: 36px; }
	.prize9 { left: 44%; bottom: 32px; }
	.prize10 { left: 56%; bottom: 30px; }
	.back { color: #8b0045; text-decoration: none; font-weight: 600; }
	.back:hover { text-decoration: underline; }
</style>
