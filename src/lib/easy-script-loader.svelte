<script lang="ts" context="module">
	declare var window: any;
</script>

<script lang="ts">
	import { BROWSER } from 'esm-env';
	import { createEventDispatcher, onMount } from 'svelte';
	import watchWindowValue from '@cloudparker/easy-window-watcher';

	export let scriptUrl: string = '';
	export let styleUrl: string = '';
	export let scriptName: string;

	let dispatch = createEventDispatcher();

	onMount(() => {
		watchWindowValue(scriptName).then((value) => {
			dispatch('load', value);
		});
	});
</script>

<svelte:head>
	{#if BROWSER && scriptName}
		{#if !window[scriptName] && scriptUrl}
			<script type="text/javascript" src={scriptUrl}></script>
			{#if styleUrl}
				<link href={styleUrl} rel="stylesheet" type="text/css" />
			{/if}
		{/if}
	{/if}
</svelte:head>
<slot />
