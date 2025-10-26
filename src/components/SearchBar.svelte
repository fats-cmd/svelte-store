<script lang="ts">
	export let placeholder: string = 'Search...';
</script>

<div class=" relative">
	<input
		type="text"
		class="w-70 h-8 p-3 pl-10 border focus:outline-0 bg-[#d9d9d9] text-left placeholder:text-left font-light text-sm transition-all"
		{placeholder}
	/>

	<!-- Search Icon -->
	<svg
		xmlns="http://www.w3.org/2000/svg"
		class="absolute left-3 top-1/2 transform -translate-y-1/2 w-5 h-5 text-gray-700"
		fill="none"
		viewBox="0 0 24 24"
		stroke="currentColor"
	>
		<path
			stroke-linecap="round"
			stroke-linejoin="round"
			stroke-width="2"
			d="M21 21l-4.35-4.35M10.5 18a7.5 7.5 0 1 1 0-15 7.5 7.5 0 0 1 0 15z"
		/>
	</svg>
</div>
