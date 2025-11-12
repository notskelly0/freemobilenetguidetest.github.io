<script>
	import '../app.css';
	
	let { children } = $props();
</script>

<svelte:head>
	<meta property="og:title" content="Hogyan legyél ingyenélő" />
	<meta property="og:description" content="Ingyen mobilnet tutorial" />
	<meta property="og:type" content="website" />
	<meta property="og:url" content="https://notskelly0.github.io" />
</svelte:head>

{@render children()}
