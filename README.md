# easy-script-loader-svelte

Simple way to load external script which will work on browser side.

# Install 

```
npm i @cloudparker/easy-script-loader-svelte --save-dev
```

# Screenshot

<img src="https://raw.githubusercontent.com/paramanandapradhan/easy-script-loader-svelte/main/static/easy-script-loader-svelte.png.webp" alt="Screenshot" width="300">

# Usage

```ts
<script lang="ts">
	import EasyScriptLoader from '@cloudparker/easy-script-loader-svelte';

	let swiperRef: HTMLDivElement;

	function handleSwipperLoad(ev: CustomEvent) {
		let Swiper: any = ev.detail;
		const swiper = new Swiper(swiperRef, {
			slidesPerView: 3,
			spaceBetween: 8,
			freeMode: true,
			pagination: {
				clickable: true
			}
		});
	}
</script>


<div>
	<EasyScriptLoader
		scriptName="Swiper"
		scriptUrls={["https://cdn.jsdelivr.net/npm/swiper@9/swiper-bundle.min.js"]}
		styleUrls={["https://cdn.jsdelivr.net/npm/swiper@9/swiper-bundle.min.css"]}
		onLoad={handleSwipperLoad}
	></EasyScriptLoader>

	<div class="swiper" bind:this={swiperRef}>
		<div class="swiper-wrapper">
			<div class="swiper-slide">Slide 1</div>
			<div class="swiper-slide">Slide 2</div>
			<div class="swiper-slide">Slide 3</div>
			<div class="swiper-slide">Slide 4</div>
			<div class="swiper-slide">Slide 5</div>
			<div class="swiper-slide">Slide 6</div>
			<div class="swiper-slide">Slide 7</div>
			<div class="swiper-slide">Slide 8</div>
			<div class="swiper-slide">Slide 9</div>
			<div class="swiper-slide">Slide 10</div>
		</div>
	</div>
</div>

<style>
	.swiper {
		width: 600px;
		height: 300px;
		border: 1px solid grey;
	}
	.swiper-slide {
		display: flex;
		align-items: center;
		justify-content: center;
		background-color: aqua;
	}
</style>

```

