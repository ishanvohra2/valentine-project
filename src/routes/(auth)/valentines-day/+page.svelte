<script>
	let revealed = false;

	function isValentinesDayUnlocked() {
		const now = new Date();
		const feb14 = new Date(now.getFullYear(), 1, 14, 0, 0, 0);
		return now >= feb14;
	}

	let unlocked = $derived(isValentinesDayUnlocked());
</script>

<svelte:head>
	<title>Valentine's Day — Valentine Week</title>
</svelte:head>

<main class="game">
	<h1>❤️ Valentine's Day</h1>
	{#if !unlocked}
		<p class="instruction">This page is available from Feb 14.</p>
		<a href="/days" class="back">← Back to days</a>
	{:else if !revealed}
		<p class="instruction">Open your heart to see the message.</p>
		<button class="open" onclick={() => (revealed = true)}>💝 Open</button>
	{:else}
		<img src="/couple.jpg" alt="Us" class="couple-img" />
		<p class="message">I love you! Happy Valentine's Day! 💕</p>
	{/if}
	{#if unlocked}
		<a href="/days" class="back">← Back to days</a>
	{/if}
</main>

<style>
	.game {
		min-height: 100vh;
		padding: 2rem 1rem;
		background: linear-gradient(180deg, #ff1493 0%, #ff69b4 50%, #ffd1dc 100%);
		text-align: center;
	}
	h1 { color: white; text-shadow: 0 1px 2px rgba(0,0,0,0.2); margin-bottom: 1rem; }
	.instruction { color: rgba(255,255,255,0.95); margin-bottom: 2rem; }
	.open {
		font-size: 1.5rem;
		padding: 1rem 2rem;
		min-height: 44px;
		background: white;
		color: #ff1493;
		border: none;
		border-radius: 16px;
		cursor: pointer;
		font-weight: 600;
	}
	.open:hover { background: #fff0f5; }
	.couple-img {
		display: block;
		width: 100%;
		max-width: 360px;
		height: auto;
		border-radius: 16px;
		margin: 0 auto 1.5rem;
		box-shadow: 0 8px 24px rgba(0,0,0,0.2);
	}
	.message {
		font-size: 1.75rem;
		color: white;
		font-weight: 600;
		text-shadow: 0 1px 3px rgba(0,0,0,0.2);
		margin-bottom: 2rem;
	}
	.back { color: white; text-decoration: none; font-weight: 600; }
	.back:hover { text-decoration: underline; }
</style>
