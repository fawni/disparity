<script lang="ts">
import favicon from '../favicon.svg';
import '@styles/app.scss'

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

{@render children?.()}
