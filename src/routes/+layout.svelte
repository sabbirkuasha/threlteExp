<script>
	import '../app.postcss';

	import { page } from '$app/stores';

	// $: console.log($page);
	// $: console.log($page.url.pathname);
</script>

<svelte:head>
	<title>Tutorial {$page.url.pathname != '/' ? `| ${$page.url.pathname.slice(1)}` : ''}</title>
</svelte:head>

<slot />
