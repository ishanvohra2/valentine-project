<script>
	import { onMount } from 'svelte';
	import { goto } from '$app/navigation';
	import { base } from '$app/paths';

	let { children } = $props();

	onMount(() => {
		if (sessionStorage.getItem('valentine-auth') !== '1') goto(base || '/');
	});
</script>

{@render children()}
