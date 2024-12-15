<script>
	import { Button } from '$lib/components/ui/button';
	import { mode, setMode, toggleMode } from 'mode-watcher';

	let count = 0;
	function handleClick() {
		return count++;
	}
</script>

<!-- <h1>Welcome to SvelteKit</h1>
<p>Visit <a href="https://svelte.dev/docs/kit">svelte.dev/docs/kit</a> to read the documentation</p>
<Button on:click={toggleMode}>
	{$mode}
</Button> -->
