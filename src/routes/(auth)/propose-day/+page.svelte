<script>
	import { onMount } from 'svelte';
	import { base } from '$app/paths';

	let answered = $state(false);
	let noClicks = $state(0);
	let noX = $state(50);
	let noY = $state(50);
	let containerEl;
	const MAX_NO_CLICKS = 10;

	function sayYes() {
		answered = true;
	}

	function clickNo() {
		noClicks += 1;
		if (noClicks >= MAX_NO_CLICKS) return;
	}

	function handleMouseMove(e) {
		if (answered || noClicks >= MAX_NO_CLICKS || !containerEl) return;
		const rect = containerEl.getBoundingClientRect();
		const mx = (e.clientX - rect.left) / rect.width * 100;
		const my = (e.clientY - rect.top) / rect.height * 100;
		const dist = Math.hypot(mx - noX, my - noY);
		if (dist < 28) {
			const angle = Math.atan2(my - noY, mx - noX);
			noX = noX + Math.cos(angle) * 22;
			noY = noY + Math.sin(angle) * 22;
			noX = Math.max(8, Math.min(92, noX));
			noY = Math.max(10, Math.min(90, noY));
		}
	}

	let yesScale = $derived(1 + noClicks * 0.12);
	let showNo = $derived(noClicks < MAX_NO_CLICKS);

	onMount(() => {
		noX = 55;
		noY = 55;
	});
</script>

<svelte:head>
	<title>Propose Day — Valentine Week</title>
</svelte:head>

<main class="game">
	<h1>💍 Propose Day</h1>
	{#if !answered}
		<p class="instruction">Will you be my Valentine?</p>
		<div
			class="buttons"
			bind:this={containerEl}
			onmousemove={handleMouseMove}
			role="presentation"
		>
			<button
				class="yes"
				style="transform: translate(-50%, -50%) scale({yesScale});"
				onclick={sayYes}
			>
				Yes 💕
			</button>
			{#if showNo}
				<button
					class="no"
					style="left: {noX}%; top: {noY}%; transform: translate(-50%, -50%);"
					onclick={clickNo}
				>
					No
				</button>
			{/if}
		</div>
	{:else}
		<p class="result yes">I knew it! Love you! ❤️</p>
	{/if}
	<a href={base + '/days'} class="back">← Back to days</a>
</main>

<style>
	.game {
		min-height: 100vh;
		padding: 2rem 1rem;
		background: linear-gradient(180deg, #fff0f5 0%, #ffd1dc 100%);
		text-align: center;
	}
	h1 { color: #8b0045; margin-bottom: 1rem; }
	.instruction { color: #5c0032; margin-bottom: 2rem; font-size: 1.25rem; }
	.buttons {
		position: relative;
		height: 220px;
		max-width: 400px;
		margin: 0 auto 2rem;
	}
	.yes {
		position: absolute;
		left: 50%;
		top: 50%;
		background: #ff69b4;
		color: white;
		border: none;
		padding: 0.75rem 2rem;
		border-radius: 12px;
		font-size: 1.1rem;
		cursor: pointer;
		font-weight: 600;
		z-index: 1;
		transition: transform 0.2s ease-out;
	}
	.no {
		position: absolute;
		background: #eee;
		color: #333;
		border: 2px solid #ccc;
		padding: 0.75rem 2rem;
		border-radius: 12px;
		font-size: 1.1rem;
		cursor: pointer;
		z-index: 2;
		transition: left 0.12s ease-out, top 0.12s ease-out;
		min-height: 44px;
	}
	.result { font-size: 1.25rem; margin-bottom: 1rem; color: #8b0045; font-weight: 600; }
	.back { color: #8b0045; text-decoration: none; font-weight: 600; display: inline-block; margin-top: 1rem; }
	.back:hover { text-decoration: underline; }
</style>
