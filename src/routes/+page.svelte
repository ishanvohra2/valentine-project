<script>
	import { onMount } from 'svelte';
	import { goto } from '$app/navigation';
	import { base } from '$app/paths';
	import { browser } from '$app/environment';
	import { PUBLIC_PASSWORD } from '$env/static/public';

	let password = '';
	let error = '';

	function handleSubmit(e) {
		error = '';
		e.preventDefault();
		if (password === PUBLIC_PASSWORD) {
			if (browser) sessionStorage.setItem('valentine-auth', '1');
			goto(`${base}/days`);
		} else {
			error = 'Wrong password. Try again.';
		}
	}

	onMount(() => {
		if (sessionStorage.getItem('valentine-auth') === '1') goto(`${base}/days`);
	});
</script>

<main class="login">
	<h1>Valentine Week</h1>
	<form onsubmit={handleSubmit}>
		<label for="password">Password</label>
		<input
			id="password"
			type="password"
			bind:value={password}
			placeholder="Enter password"
			autocomplete="off"
		/>
		{#if error}
			<p class="error">{error}</p>
		{/if}
		<button type="submit">Enter</button>
	</form>
</main>

<style>
	.login {
		min-height: 100vh;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		padding: 1rem;
		background: linear-gradient(135deg, #ffd1dc 0%, #ffb6c1 50%, #ff69b4 100%);
	}
	h1 {
		color: #8b0045;
		margin-bottom: 2rem;
		font-size: 2.5rem;
	}
	form {
		display: flex;
		flex-direction: column;
		gap: 0.75rem;
		background: rgba(255, 255, 255, 0.9);
		padding: 2rem;
		border-radius: 12px;
		box-shadow: 0 4px 20px rgba(139, 0, 69, 0.2);
		min-width: 280px;
	}
	label {
		font-weight: 600;
		color: #5c0032;
	}
	input {
		padding: 0.6rem 0.8rem;
		border: 2px solid #ffb6c1;
		border-radius: 8px;
		font-size: 16px; /* prevents zoom on focus on iOS */
	}
	input:focus {
		outline: none;
		border-color: #ff69b4;
	}
	.error {
		color: #c00;
		font-size: 0.9rem;
		margin: 0;
	}
	button {
		padding: 0.75rem 1.5rem;
		background: #ff69b4;
		color: white;
		border: none;
		border-radius: 8px;
		font-size: 1rem;
		font-weight: 600;
		cursor: pointer;
		margin-top: 0.5rem;
	}
	button:hover {
		background: #ff1493;
	}
</style>
