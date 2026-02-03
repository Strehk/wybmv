<script lang="ts">
	import '../app.css';
	import { onMount } from 'svelte';
	import { i18n } from '$lib/i18n';
	import { base } from '$app/paths';

	let { children } = $props();

	let isDark = $state(false);

	onMount(() => {
		// Check system preference
		const mediaQuery = window.matchMedia('(prefers-color-scheme: dark)');
		isDark = mediaQuery.matches;

		// Listen for changes
		const handler = (e: MediaQueryListEvent) => {
			isDark = e.matches;
		};
		mediaQuery.addEventListener('change', handler);

		return () => mediaQuery.removeEventListener('change', handler);
	});
</script>

<svelte:head>
	<meta name="description" content="A romantic way to ask someone to be your Valentine" />
</svelte:head>

<div class="min-h-screen flex flex-col" class:dark={isDark}>
	<main class="flex-grow flex flex-col items-center justify-center bg-valentine-light-bg dark:bg-valentine-dark-bg text-valentine-light-text dark:text-valentine-dark-text transition-colors duration-300">
		{@render children()}
	</main>

	<footer class="py-4 text-center text-xs opacity-50 bg-valentine-light-bg dark:bg-valentine-dark-bg text-valentine-light-text dark:text-valentine-dark-text">
		<p>
			<a href="{base}/config" class="hover:underline">{$i18n.t('footer.createYourOwn')}</a> | {$i18n.t('footer.madeWith')} ❤️ | © 2025 | <a href="https://github.com/Strehk" class="hover:underline" target="_blank" rel="noopener noreferrer">{$i18n.t('footer.github')}</a>
		</p>
	</footer>
</div>
