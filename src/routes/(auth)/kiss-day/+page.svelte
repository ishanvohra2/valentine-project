<script>
	let kisses = [];
	let faceEl;

	function addKiss(e) {
		if (!faceEl) return;
		const rect = faceEl.getBoundingClientRect();
		const x = ((e.clientX - rect.left) / rect.width) * 100;
		const y = ((e.clientY - rect.top) / rect.height) * 100;
		kisses = [...kisses, { id: kisses.length, x: x + (Math.random() - 0.5) * 20, y: y + (Math.random() - 0.5) * 20 }];
	}
</script>

<svelte:head>
	<title>Kiss Day — Valentine Week</title>
</svelte:head>

<main class="game">
	<h1>💋 Kiss Day</h1>
	<p class="instruction">Click on the face to give a kiss mark!</p>
	<div
		class="face-wrap"
		bind:this={faceEl}
		onclick={addKiss}
		onkeydown={(e) => { if (e.key === 'Enter' || e.key === ' ') addKiss(e); }}
		role="button"
		tabindex="0"
	>
		<img src="/me.png" alt="Me" class="face" />
		{#each kisses as kiss (kiss.id)}
			<span class="kiss" style="left: {kiss.x}%; top: {kiss.y}%;">💋</span>
		{/each}
	</div>
	<p class="count">{kisses.length} kiss{kisses.length === 1 ? '' : 'es'} 💕</p>
	<a href="/days" class="back">← Back to days</a>
</main>

<style>
	.game {
		min-height: 100vh;
		padding: 2rem 1rem;
		background: linear-gradient(180deg, #ffd1dc 0%, #ff69b4 100%);
		text-align: center;
	}
	h1 { color: #8b0045; margin-bottom: 0.5rem; }
	.instruction { color: #5c0032; margin-bottom: 1rem; }
	.face-wrap {
		position: relative;
		display: inline-block;
		cursor: pointer;
		margin-bottom: 1rem;
	}
	.face {
		display: block;
		width: 200px;
		height: 200px;
		object-fit: cover;
		border-radius: 50%;
		user-select: none;
		max-width: 100%;
	}
	.kiss {
		position: absolute;
		transform: translate(-50%, -50%);
		font-size: 2rem;
		pointer-events: none;
		animation: pop 0.3s ease-out;
	}
	@keyframes pop {
		from { transform: translate(-50%, -50%) scale(0); opacity: 0; }
		to { transform: translate(-50%, -50%) scale(1); opacity: 1; }
	}
	.count { font-weight: 600; color: #5c0032; margin-bottom: 1rem; }
	.back { color: #8b0045; text-decoration: none; font-weight: 600; }
	.back:hover { text-decoration: underline; }
</style>
