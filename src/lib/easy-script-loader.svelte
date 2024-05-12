<script lang="ts" context="module">
	declare var window: any;
</script>

<script lang="ts">
	import { BROWSER } from 'esm-env';
	import { createEventDispatcher, onMount } from 'svelte';
	import { getValue, watchWindowValue } from '@cloudparker/easy-window-watcher';

	export let scriptName: string;
	export let scriptUrl: string | string[] = '';
	export let styleUrl: string | string[] = '';
	export let scriptType: string = 'text/javascript';
	export let styleType: string = 'text/css';

	let dispatch = createEventDispatcher();

	let scriptUrls: string[] = [];
	let styleUrls: string[] = [];

	function checkWindowValue() {
		let val = getValue(window, scriptName);
		return !!val;
	}

	function prepare(scriptUrl: string | string[], styleUrl: string | string[]) {
		if (scriptUrl) {
			if (Array.isArray(scriptUrl)) {
				scriptUrls = scriptUrl || [];
			} else {
				scriptUrls = [scriptUrl];
			}
		} else {
			scriptUrls = [];
		}

		if (styleUrl) {
			if (Array.isArray(styleUrl)) {
				styleUrls = styleUrl || [];
			} else {
				styleUrls = [styleUrl];
			}
		} else {
			styleUrls = [];
		}

		// console.log('prepare', scriptUrls, styleUrls);
	}

	onMount(() => {
		watchWindowValue(scriptName).then((value) => {
			dispatch('load', value);
		});
	});

	$: prepare(scriptUrl, styleUrl);
</script>

<svelte:head>
	{#if BROWSER && scriptName}
		{#if !checkWindowValue()}
			{#each scriptUrls as script}
				<script src={script} type={scriptType}></script>
			{/each}

			{#each styleUrls as style}
				<link href={style} rel="stylesheet" type={styleType} />
			{/each}
		{/if}
	{/if}
</svelte:head>
<!-- <slot /> -->
