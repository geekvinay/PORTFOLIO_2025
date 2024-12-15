<script>
// @ts-nocheck

	import { onMount } from 'svelte';
	import Typed from 'typed.js';

	export let strings = [];
	export let typeSpeed = 100;
	export let startDelay = 500;
	export let backSpeed = 100;
	export let backDelay = 5000;
	export let showCursor = false;
	export let loop = true;

	let typedElement;

	onMount(() => {
		const options = {
			strings,
			typeSpeed,
			startDelay,
			backSpeed,
			backDelay,
			showCursor,
			loop
		};

		const typed = new Typed(typedElement, options);

		// Clean up when component is destroyed
		return () => typed.destroy();
	});
</script>

<span bind:this={typedElement}></span>
