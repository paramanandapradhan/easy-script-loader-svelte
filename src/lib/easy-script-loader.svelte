<script lang="ts" context="module">
	declare var window: any;
</script>

<script lang="ts">
	import { BROWSER } from 'esm-env';
	import { getValue, watchWindowValue } from '@cloudparker/easy-window-watcher';

	type PropsType = {
		scriptName: string;
		scriptUrl?: string | string[];
		styleUrl?: string | string[];
		scriptType?: string;
		styleType?: string;
		onload?: (lib: any) => void;
	};

	let {
		scriptName,
		scriptUrl = '',
		styleUrl = '',
		styleType = 'text/css',
		scriptType = 'text/javascript',
		onload
	}: PropsType = $props();

	let scriptUrls: string[] = $state([]);
	let styleUrls: string[] = $state([]);

	let isSent: boolean = false;

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

	$effect(() => {
		watchWindowValue(scriptName).then((value) => {
			if (!isSent) {
				onload && onload(value);
			}
		});
	});

	$effect(() => {
		prepare(scriptUrl, styleUrl);
	});
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
