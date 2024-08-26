<script lang="ts" context="module">
 
</script>

<script lang="ts">
	import { watchWindowValue } from '@cloudparker/easy-window-watcher';

	type PropsType = {
		scriptName: string;
		scriptUrl?: string;
		styleUrl?: string;
		scriptUrls?: string[];
		styleUrls?: string[];
		scriptType?: string;
		styleType?: string;
		onLoad?: (lib: any) => void;
	};

	let {
		scriptName,
		scriptUrl = '',
		styleUrl = '',
		scriptUrls = [],
		styleUrls = [],
		styleType = 'text/css',
		scriptType = 'text/javascript',
		onLoad
	}: PropsType = $props();

	let isSent: boolean = $state(false);

	$effect(() => {
		watchWindowValue(scriptName).then((value) => {
			if (!isSent) {
				// console.log('watchWindowValue', value);
				onLoad && onLoad(value);
			}
		});
	});
</script>

<svelte:head>
	{#if scriptUrl}
		<script src={scriptUrl} type={scriptType}></script>
	{/if}
	{#if styleUrl}
		<link href={styleUrl} rel="stylesheet" type={styleType} />
	{/if}
	{#if scriptUrls?.length}
		{#each scriptUrls as scriptUrl}
			<script src={scriptUrl} type={scriptType}></script>
		{/each}
	{/if}
	{#if styleUrls?.length}
		{#each styleUrls as styleUrl}
			<link href={styleUrl} rel="stylesheet" type={styleType} />
		{/each}
	{/if}
</svelte:head>
