<script lang="ts" context="module">
	declare var window: any;
</script>

<script lang="ts">
	import { BROWSER } from 'esm-env';
	import { createEventDispatcher, onMount } from 'svelte';
	import { getValue, watchWindowValue } from '@cloudparker/easy-window-watcher';

	export let scriptUrl: string | string[] = '';
	export let styleUrl: string | string[] = '';
	export let scriptName: string;

	let dispatch = createEventDispatcher();

	function checkWindowValue() {
		return !!getValue(window, scriptName);
	}

	onMount(() => {
		watchWindowValue(scriptName).then((value) => {
			dispatch('load', value);
		});
	});
</script>

<svelte:head>
	{#if BROWSER && scriptName}
		{#if checkWindowValue() && scriptUrl}
			{#if Array.isArray(scriptUrl)}
				{#each scriptUrl as script}
					{#if scriptUrl}
						<script type="text/javascript" src={script}></script>
					{/if}
				{/each}
			{:else if scriptUrl}
				<script type="text/javascript" src={scriptUrl}></script>
			{/if}
			{#if Array.isArray(styleUrl)}
				{#each styleUrl as style}
					{#if styleUrl}
						<link href={style} rel="stylesheet" type="text/css" />
					{/if}
				{/each}
			{:else if styleUrl}
				<link href={styleUrl} rel="stylesheet" type="text/css" />
			{/if}
		{/if}
	{/if}
</svelte:head>
<slot />
