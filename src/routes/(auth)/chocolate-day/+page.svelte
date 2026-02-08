<script>
	const emojis = ['🍫', '🍬', '🍩', '🎂', '🍪', '🧁', '🍭', '🎀', '💝', '🫐', '🍓', '🍒'];
	let cards = [...emojis, ...emojis]
		.map((e, i) => ({ id: i, emoji: e, flipped: false, matched: false }))
		.sort(() => Math.random() - 0.5);

	let firstIndex = null;
	let locked = false;

	function flip(index) {
		const card = cards[index];
		if (locked || card.flipped || card.matched) return;
		cards = cards.map((c, i) => (i === index ? { ...c, flipped: true } : c));
		if (firstIndex === null) {
			firstIndex = index;
			return;
		}
		locked = true;
		const firstCard = cards[firstIndex];
		if (firstCard.emoji === card.emoji) {
			cards = cards.map((c, i) =>
				i === index || i === firstIndex ? { ...c, matched: true } : c
			);
			firstIndex = null;
			locked = false;
		} else {
			setTimeout(() => {
				cards = cards.map((c, i) =>
					i === index || i === firstIndex ? { ...c, flipped: false } : c
				);
				firstIndex = null;
				locked = false;
			}, 600);
		}
	}
</script>

<svelte:head>
	<title>Chocolate Day — Valentine Week</title>
</svelte:head>

<main class="game">
	<h1>🍫 Chocolate Day</h1>
	<p class="instruction">Match the pairs!</p>
	<div class="grid">
		{#each cards as card, i (card.id)}
			<button
				class="card"
				class:flipped={card.flipped}
				class:matched={card.matched}
				onclick={() => flip(i)}
				disabled={card.matched}
			>
				<span class="front">{card.emoji}</span>
				<span class="back">?</span>
			</button>
		{/each}
		<div class="card empty" aria-hidden="true"></div>
	</div>
	{#if cards.every((c) => c.matched)}
		<p class="done">All pairs matched! 🍫💕</p>
	{/if}
	<a href="/days" class="back">← Back to days</a>
</main>

<style>
	.game {
		min-height: 100vh;
		padding: 2rem 1rem;
		background: linear-gradient(180deg, #fff8f0 0%, #ffe4c9 100%);
		text-align: center;
	}
	h1 { color: #5c3317; margin-bottom: 0.5rem; }
	.instruction { color: #5c0032; margin-bottom: 1.5rem; }
	.grid {
		display: grid;
		grid-template-columns: repeat(5, 1fr);
		gap: 0.4rem;
		max-width: 340px;
		margin: 0 auto 2rem;
	}
	.card {
		aspect-ratio: 1;
		border: 2px solid #d2691e;
		border-radius: 8px;
		background: #ffd1dc;
		font-size: 1.4rem;
		cursor: pointer;
		display: flex;
		align-items: center;
		justify-content: center;
	}
	.card.empty { visibility: hidden; pointer-events: none; }
	.card .back { display: block; }
	.card .front { display: none; }
	.card.flipped .back { display: none; }
	.card.flipped .front { display: block; }
	.card.matched { opacity: 0.7; cursor: default; }
	.done { color: #8b0045; font-weight: 600; margin-bottom: 1rem; }
	.back { color: #8b0045; text-decoration: none; font-weight: 600; }
	.back:hover { text-decoration: underline; }
</style>
