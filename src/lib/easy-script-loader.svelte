<script lang="ts">
	import { getValue } from '@cloudparker/easy-window-watcher';
	import { BROWSER } from 'esm-env';
	type PropsType = {
		scriptUrl?: string | string[];
		styleUrl?: string | string[];
		scriptType?: string;
		styleType?: string;
		scriptName: string;
		onLoad?: (lib: any) => void;
	};
	let {
		scriptUrl = '',
		styleUrl = '',
		styleType = 'text/css',
		scriptType = 'text/javascript',
		scriptName,
		onLoad
	}: PropsType = $props();

	let isCreated: boolean = $state(false);
	let lib: any = $state(null);

	function addScript(jsUrl: string): Promise<void> {
		return new Promise((resolve, reject) => {
			const scriptElement = document.createElement('script');
			scriptElement.src = jsUrl;
			scriptElement.async = true;
			scriptElement.type = scriptType;
			scriptElement.crossOrigin = 'anonymous';
			scriptElement.onload = () => resolve();
			scriptElement.onerror = () => reject(new Error(`Failed to load script: ${jsUrl}`));
			document.head.appendChild(scriptElement);
		});
	}

	function addLink(cssUrl: string): Promise<void> {
		return new Promise((resolve, reject) => {
			const linkElement = document.createElement('link');
			linkElement.rel = 'stylesheet';
			linkElement.href = cssUrl;
			linkElement.type = styleType;
			linkElement.onload = () => resolve();
			linkElement.onerror = () => reject(new Error(`Failed to load style: ${cssUrl}`));
			document.head.appendChild(linkElement);
		});
	}

	async function createTags() {
		if (BROWSER && window && !isCreated) {
			lib = getValue(window, scriptName);
			isCreated = true;
			if (!lib) {
				let tasks: Promise<void>[] = [];

				if (Array.isArray(styleUrl)) {
					tasks.push(
						...styleUrl.map(async (url) => {
							return addLink(url);
						})
					);
				} else if (styleUrl) {
					tasks.push(addLink(styleUrl));
				}
				if (Array.isArray(scriptUrl)) {
					tasks.push(
						...scriptUrl.map(async (url) => {
							return addScript(url);
						})
					);
				} else if (scriptUrl) {
					tasks.push(addScript(scriptUrl));
				}
				try {
					await Promise.all(tasks);
				} catch (error) {
					console.error(error);
				}
			}

			console.log('Last', { window, scriptName });
			lib = getValue(window, scriptName);
			if (lib) {
				onLoad && onLoad(lib);
			}
		}
	}

	$effect(() => {
		if (!isCreated) {
			createTags();
		}
	});
</script>

<div></div>
