<script>
	import { base } from '$app/paths';

	// Valentine Week 2025: each day unlocks at 00:00 on its date (Feb = month 1)
	const days = [
		{ name: 'Rose Day', path: '/rose-day', emoji: '🌹', date: 7 },
		{ name: 'Propose Day', path: '/propose-day', emoji: '💍', date: 8 },
		{ name: 'Chocolate Day', path: '/chocolate-day', emoji: '🍫', date: 9 },
		{ name: 'Teddy Day', path: '/teddy-day', emoji: '🧸', date: 10 },
		{ name: 'Promise Day', path: '/promise-day', emoji: '🤝', date: 11 },
		{ name: 'Hug Day', path: '/hug-day', emoji: '🤗', date: 12 },
		{ name: 'Kiss Day', path: '/kiss-day', emoji: '💋', date: 13 },
		{ name: "Valentine's Day", path: '/valentines-day', emoji: '❤️', date: 14 }
	];

	function isUnlocked(febDate) {
		const now = new Date();
		const year = now.getFullYear();
		const unlock = new Date(year, 1, febDate, 0, 0, 0);
		return now >= unlock;
	}

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
				{#if !isUnlocked(day.date)}
					<span class="day-link disabled" title="Available from Feb {day.date}" role="link" aria-disabled="true">
						<span class="emoji">{day.emoji}</span>
						<span class="name">{day.name}</span>
						<span class="locked">— Feb {day.date}</span>
					</span>
				{:else}
					<a href={base + day.path} class="day-link">
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
