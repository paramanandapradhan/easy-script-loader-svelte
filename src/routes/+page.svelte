<script lang="ts">
	import EasyScriptLoader from '../lib/easy-script-loader.svelte';

	let swiperRef: HTMLDivElement;

	function handleSwipperLoad(lib: any) {
		let Swiper: any = lib;
		console.log('Swiper = ', Swiper);
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
			<div class="swiper-slide">Slide 101</div>
		</div>
	</div>
</div>

<EasyScriptLoader
	scriptName="Swiper"
	scriptUrl={['https://cdn.jsdelivr.net/npm/swiper@9/swiper-bundle.min.js']}
	styleUrl={['https://cdn.jsdelivr.net/npm/swiper@9/swiper-bundle.min.css']}
	onLoad={handleSwipperLoad}
></EasyScriptLoader>

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
