<!-- Right Carousel Section -->
<!-- <div class="md:w-2/3">
			<div
                use:emblaCarouselSvelte="{{options}}"
				
				class="overflow-hidden"
			>
				<div class="flex">
					{#each slides as slide}
						<div class="flex-[0_0_100%] md:flex-[0_0_50%] px-2">
							<img
								src={slide.img}
								alt={slide.alt}
								class="w-full h-[400px] object-cover rounded-lg shadow-sm"
							/>
						</div>
					{/each}
				</div>
			</div>
		</div>
	</div>
</section> -->

<script lang="ts">
	import emblaCarouselSvelte from 'embla-carousel-svelte';

	let options = { loop: true, align: 'start', dragFree: false };
	let emblaApi;

	function onInit(event: CustomEvent) {
		emblaApi = event.detail;
		console.log(emblaApi.slideNodes()); // Access API
	}
</script>

<!-- <script lang="ts">
	import emblaCarouselSvelte  from 'embla-carousel-svelte';

    let options = { loop: true, align: 'start', dragFree: false };  

	const slides = [
		{
			id: 1,
			img: '/images/collection1.jpg',
			alt: 'Beige pants outfit'
		},
		{
			id: 2,
			img: '/images/collection2.jpg',
			alt: 'Black embroidered shirt'
		}
	];
</script>

<section class="w-full bg-[#f8f8f8] py-10">
	<div class="max-w-7xl mx-auto px-6 md:flex md:items-center md:justify-between gap-8">
		<!-- Left Text Section -->
<!-- <div class="md:w-1/3 mb-8 md:mb-0"> -->
<!-- <h2 class="text-4xl font-bold leading-tight text-gray-900">
				NEW<br />COLLECTION
			</h2>
			<p class="mt-3 text-gray-700">Summer<br />2024</p>

			<button
				class="group mt-6 flex items-center gap-3 border border-gray-800 px-5 py-2 text-sm uppercase tracking-wider hover:bg-gray-900 hover:text-white transition-all duration-300"
			>
				Go To Shop
				<span
					class="inline-block transform translate-x-0 group-hover:translate-x-1 transition-transform duration-300"
					>➜</span
				>
			</button>
		</div> -->
-->

<div class="embla" onemblaInit={onInit} use:emblaCarouselSvelte>
	<div class="embla__container max-w-16 mx-auto px-4">
		<div class="embla__slide">Slide 1</div>
		<div class="embla__slide">Slide 2</div>
		<div class="embla__slide">Slide 3</div>
	</div>
</div>

<style>
	.embla {
		overflow: hidden;

	}
	.embla__container {
		display: flex;
        justify-content: center;
        align-items: center;
	}
	.embla__slide {
		flex: 0 0 100%;
		min-width: 0;
	}
</style>
