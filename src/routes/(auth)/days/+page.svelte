<script>
	const days = [
		{ name: 'Rose Day', path: '/rose-day', emoji: '🌹' },
		{ name: 'Propose Day', path: '/propose-day', emoji: '💍' },
		{ name: 'Chocolate Day', path: '/chocolate-day', emoji: '🍫' },
		{ name: 'Teddy Day', path: '/teddy-day', emoji: '🧸' },
		{ name: 'Promise Day', path: '/promise-day', emoji: '🤝' },
		{ name: 'Hug Day', path: '/hug-day', emoji: '🤗' },
		{ name: 'Kiss Day', path: '/kiss-day', emoji: '💋' },
		{ name: "Valentine's Day", path: '/valentines-day', emoji: '❤️', valentinesDay: true }
	];

	function isValentinesDayUnlocked() {
		const now = new Date();
		const year = now.getFullYear();
		const feb14 = new Date(year, 1, 14, 0, 0, 0); // Feb 14 00:00 local
		return now >= feb14;
	}

	let valentinesEnabled = $derived(isValentinesDayUnlocked());
</script>

<svelte:head>
	<title>Valentine Week — Choose a day</title>
</svelte:head>

<main class="days">
	<h1>Valentine Week</h1>
	<p class="subtitle">Pick a day to play</p>
	<ul class="day-list">
		{#each days as day}
			<li>
				{#if day.valentinesDay && !valentinesEnabled}
					<span class="day-link disabled" title="Available from Feb 14">
						<span class="emoji">{day.emoji}</span>
						<span class="name">{day.name}</span>
						<span class="locked">— Feb 14</span>
					</span>
				{:else}
					<a href={day.path} class="day-link">
						<span class="emoji">{day.emoji}</span>
						<span class="name">{day.name}</span>
					</a>
				{/if}
			</li>
		{/each}
	</ul>
</main>

<style>
	.days {
		min-height: 100vh;
		padding: 2rem 1rem;
		background: linear-gradient(135deg, #ffd1dc 0%, #ffb6c1 50%, #ff69b4 100%);
	}
	h1 {
		text-align: center;
		color: #8b0045;
		margin-bottom: 0.5rem;
		font-size: 2rem;
	}
	.subtitle {
		text-align: center;
		color: #5c0032;
		margin-bottom: 2rem;
	}
	.day-list {
		list-style: none;
		padding: 0;
		margin: 0 auto;
		max-width: 360px;
		display: flex;
		flex-direction: column;
		gap: 0.75rem;
	}
	.day-link {
		display: flex;
		align-items: center;
		gap: 1rem;
		padding: 1rem 1.25rem;
		min-height: 48px;
		background: rgba(255, 255, 255, 0.95);
		border-radius: 12px;
		text-decoration: none;
		color: #5c0032;
		font-weight: 600;
		box-shadow: 0 2px 12px rgba(139, 0, 69, 0.15);
		transition: transform 0.2s, box-shadow 0.2s;
	}
	.day-link:hover {
		transform: translateY(-2px);
		box-shadow: 0 4px 20px rgba(139, 0, 69, 0.25);
	}
	.day-link.disabled {
		opacity: 0.6;
		cursor: not-allowed;
		pointer-events: none;
		color: #888;
	}
	.day-link.disabled .locked {
		font-size: 0.85rem;
		color: #999;
		margin-left: auto;
	}
	.emoji {
		font-size: 1.75rem;
	}
</style>
