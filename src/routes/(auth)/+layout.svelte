<script>
	import { onMount } from 'svelte';
	import { goto } from '$app/navigation';

	let { children } = $props();

	onMount(() => {
		if (sessionStorage.getItem('valentine-auth') !== '1') goto('/');
	});
</script>

{@render children()}
