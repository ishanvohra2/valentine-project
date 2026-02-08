<script>
	let tightness = 0;
	const maxTightness = 10;
	function hug() {
		if (tightness < maxTightness) tightness += 1;
	}
</script>

<svelte:head>
	<title>Hug Day — Valentine Week</title>
</svelte:head>

<main class="game">
	<div class="click-area" role="button" tabindex="0" onclick={hug} onkeydown={(e) => (e.key === 'Enter' || e.key === ' ') && hug()}>
	<h1>🤗 Hug Day</h1>
	<p class="instruction">Click the screen to make her hug him tighter!</p>
	<div class="hug-container" style="--tight: {tightness / maxTightness};">
		<div class="figures">
			<span class="girl">👩</span>
			<span class="heart">💕</span>
			<span class="boy">👨</span>
		</div>
		<p class="tightness">Hug tightness: {tightness}/{maxTightness}</p>
	</div>
	</div>
	{#if tightness >= maxTightness}
		<p class="done">So much love! 💕</p>
	{/if}
	<a href="/days" class="back">← Back to days</a>
</main>

<style>
	.game {
		min-height: 100vh;
		padding: 2rem 1rem;
		background: linear-gradient(180deg, #fff0f5 0%, #ffd1dc 100%);
		text-align: center;
	}
	.click-area {
		cursor: pointer;
	}
	h1 { color: #8b0045; margin-bottom: 0.5rem; }
	.instruction { color: #5c0032; margin-bottom: 1.5rem; }
	.hug-container {
		max-width: 320px;
		margin: 0 auto 2rem;
		padding: 2rem;
		background: rgba(255, 255, 255, 0.7);
		border-radius: 16px;
		border: 3px solid #ffb6c1;
	}
	.figures {
		display: flex;
		align-items: center;
		justify-content: center;
		gap: calc(1rem - var(--tight, 0) * 1.2rem);
		font-size: 4rem;
		margin-bottom: 1rem;
	}
	.girl, .boy {
		transition: transform 0.2s;
	}
	.girl { transform: translateX(calc(var(--tight, 0) * 8px)); }
	.boy { transform: translateX(calc(var(--tight, 0) * -8px)); }
	.heart {
		font-size: 2.5rem;
		opacity: calc(0.7 + var(--tight, 0) * 0.3);
	}
	.tightness { font-weight: 600; color: #8b0045; margin: 0; }
	.done { font-size: 1.25rem; color: #8b0045; font-weight: 600; margin-bottom: 1rem; }
	.back { color: #8b0045; text-decoration: none; font-weight: 600; cursor: pointer; }
	.back:hover { text-decoration: underline; }
</style>
