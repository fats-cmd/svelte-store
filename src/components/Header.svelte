<script lang="ts">
	import logo from '/assets/images/logo-removebg-preview.png';
	import menuIcon from '/assets/images/menu-icon.png';

	// Simple, reusable header props
	import { tick, onDestroy } from 'svelte';
	// guard DOM access during SSR
	const isBrowser = typeof document !== 'undefined' && typeof window !== 'undefined';
	import { fade, fly } from 'svelte/transition';
	import SearchBar from './SearchBar.svelte';

	export let cartCount: number = 0;
	export let favCount: number = 0;
	export let siteName: string = 'MyStore';

	let menuOpen: boolean = false;
	let firstMobileLink: HTMLAnchorElement | null = null;

	async function openMenu() {
		menuOpen = true;
		// wait for DOM then focus first link and prevent body scroll
		await tick();
		if (isBrowser) {
			firstMobileLink?.focus();
			document.body.style.overflow = 'hidden';
		}
	}

	function closeMenu() {
		menuOpen = false;
		if (isBrowser) document.body.style.overflow = '';
	}

	function toggleMenu() {
		if (menuOpen) closeMenu();
		else openMenu();
	}

	onDestroy(() => {
		// ensure no leftover style
		if (isBrowser) document.body.style.overflow = '';
	});
</script>

<header class="sticky top-0 z-50 w-fullshadow-sm">
	<div class="max-w-7xl mx-auto flex items-center justify-between gap-3 px-4 py-3">
		<!-- hamburger: visible on small screens -->
		<button
			class="lg:hidden p-2 rounded-md text-gray-700 hover:bg-gray-200 focus:outline-none focus:ring-2 focus:ring-black cursor-pointer"
			aria-label="Toggle menu"
			aria-controls="mobile-menu"
			aria-expanded={menuOpen}
			on:click={toggleMenu}
		>
			<img src={menuIcon} alt="hamburger menu" class="h-4 w-auto" />
		</button>

		<!-- left nav: expanded width on larger screens -->
		<nav
			class="hidden lg:flex gap-6 items-center font-medium md:font-light"
			aria-label="Main navigation"
		>
			<a
				href="/"
				class="  transition-colors py-2 relative pb-2 border-b-2 border-transparent hover:border-black"
			>
				Home
			</a>
			<a
				href="/shop"
				class="transition-colors py-2 relative pb-2 border-b-2 border-transparent hover:border-black"
			>
				Collection
			</a>
			<a
				href="/about"
				class="transition-colors py-2 relative pb-2 border-b-2 border-transparent hover:border-black"
			>
				New
			</a>
		</nav>

		<!-- middle logo -->
		<div class="shrink-0 text-center lg:grow">
			<a href="/" class="inline-block" aria-label={siteName}>
				<img src={logo} alt={siteName} class="h-10 w-auto" />
			</a>
		</div>

		<!-- right icons -->
		<div class="flex items-center gap-4 justify-end shrink-0">
			<!-- Icons with hover labels -->
			<div class="group relative">
				<button
					class="relative p-2 rounded-full text-gray-700 bg-transparent hover:bg-black hover:text-white lg:group-hover:bg-black lg:group-hover:text-white focus:outline-none hover:scale-105 transition-all duration-150 flex items-center justify-center"
					aria-label="Favorites"
				>
					<svg
						xmlns="http://www.w3.org/2000/svg"
						class="w-6 h-6"
						fill="none"
						viewBox="0 0 24 24"
						stroke="currentColor"
						aria-hidden="true"
					>
						<path
							stroke-linecap="round"
							stroke-linejoin="round"
							stroke-width={1.5}
							d="M4.318 6.318a4.5 4.5 0 000 6.364L12 20.364l7.682-7.682a4.5 4.5 0 00-6.364-6.364L12 7.636l-1.318-1.318a4.5 4.5 0 00-6.364 0z"
						/>
					</svg>
					{#if favCount > 0}
						<span
							class="absolute -top-1 -right-1 bg-blue-600 text-white text-xs font-medium px-1.5 min-w-5 h-5 rounded-full flex items-center justify-center"
							aria-label={`${favCount} items in favorites`}>{favCount}</span
						>
					{/if}
				</button>
				<!-- Hover label -->
				<div
					class="hidden lg:block absolute left-1/2 top-full -translate-x-1/2 pt-2 opacity-0 group-hover:opacity-100 transition-all duration-150 pointer-events-none"
				>
					<div
						class="bg-black text-white text-sm font-medium px-3 py-1 rounded-full whitespace-nowrap"
					>
						favorites
					</div>
				</div>
			</div>

			<div class="group relative">
				<button
					class="relative p-2 rounded-full text-gray-700 bg-transparent hover:bg-black hover:text-white lg:group-hover:bg-black lg:group-hover:text-white focus:outline-none transition-all duration-150"
					aria-label="Shopping cart"
				>
					<svg
						xmlns="http://www.w3.org/2000/svg"
						class="w-6 h-6"
						fill="none"
						viewBox="0 0 24 24"
						stroke="currentColor"
						aria-hidden="true"
					>
						<path
							stroke-linecap="round"
							stroke-linejoin="round"
							stroke-width={1.5}
							d="M3 3h2l.4 2M7 13h10l4-8H5.4M7 13L5.4 5M7 13l-2.293 2.293c-.63.63-.184 1.707.707 1.707H17m0 0a2 2 0 100 4 2 2 0 000-4zm-8 2a2 2 0 11-4 0 2 2 0 014 0z"
						/>
					</svg>
					{#if cartCount > 0}
						<span
							class="absolute -top-1 -right-1 bg-blue-600 text-white text-xs font-medium px-1.5 min-w-5 h-5 rounded-full flex items-center justify-center"
							aria-label={`${cartCount} items in cart`}>{cartCount}</span
						>
					{/if}
				</button>
				<!-- Hover label -->
				<div
					class="hidden lg:block absolute left-1/2 top-full -translate-x-1/2 pt-2 opacity-0 group-hover:opacity-100 transition-all duration-150 pointer-events-none"
				>
					<div
						class="bg-black text-white text-sm font-medium px-3 py-1 rounded-full whitespace-nowrap"
					>
						cart
					</div>
				</div>
			</div>

			<div class="group relative">
				<a
					class="p-2 rounded-full text-gray-700 bg-transparent hover:bg-black hover:text-white lg:group-hover:bg-black lg:group-hover:text-white focus:outline-none transition-all duration-150 inline-flex items-center justify-center"
					href="/profile"
					aria-label="My profile"
				>
					<svg
						xmlns="http://www.w3.org/2000/svg"
						class="w-6 h-6"
						fill="none"
						viewBox="0 0 24 24"
						stroke="currentColor"
						aria-hidden="true"
					>
						<path
							stroke-linecap="round"
							stroke-linejoin="round"
							stroke-width={1.5}
							d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z"
						/>
					</svg>
				</a>
				<!-- Hover label -->
				<div
					class="hidden lg:block absolute left-1/2 top-full -translate-x-1/2 pt-2 opacity-0 group-hover:opacity-100 transition-all duration-150 pointer-events-none"
				>
					<div
						class="bg-black text-white text-sm font-medium px-3 py-1 rounded-full whitespace-nowrap"
					>
						profile
					</div>
				</div>
			</div>
		</div>
	</div>
	<!-- bottom navigation -->
	<div class="md:max-w-6xl max-w-5xl px-4 mx-auto py-6">
		<ul class="block space-y-2 capitalize list-none font-light">
			<li>
				<a
					href="#"
					class="group flex items-center gap-2 transition-all duration-300 hover:text-gray-700"
				>
					Men
					<span
						class="inline-block transform translate-x-0 opacity-0 group-hover:translate-x-1 group-hover:opacity-100 transition-all duration-300"
					>
						➜
					</span>
				</a>
			</li>

			<li>
				<a
					href="#"
					class="group flex items-center gap-2 transition-all duration-300 hover:text-gray-700"
				>
					Women
					<span
						class="inline-block transform translate-x-0 opacity-0 group-hover:translate-x-1 group-hover:opacity-100 transition-all duration-300"
					>
						➜
					</span>
				</a>
			</li>

			<li>
				<a
					href="#"
					class="group flex items-center gap-2 transition-all duration-300 hover:text-gray-700"
				>
					Kids
					<span
						class="inline-block transform translate-x-0 opacity-0 group-hover:translate-x-1 group-hover:opacity-100 transition-all duration-300"
					>
						➜
					</span>
				</a>
			</li>

			<!-- Searchbar component -->
			<SearchBar />
		</ul>
	</div>
</header>

{#if menuOpen}
	<div class="lg:hidden">
		<button
			class="fixed inset-0 bg-black/40 z-40"
			on:click={closeMenu}
			aria-label="Close mobile menu"
			transition:fade={{ duration: 200 }}
		></button>
		<div
			id="mobile-menu"
			class="fixed top-0 left-0 h-screen w-[min(320px,80vw)] bg-white z-50 shadow-lg flex flex-col"
			role="dialog"
			aria-label="Mobile menu"
			aria-modal="true"
			transition:fly={{ x: -320, duration: 300, opacity: 1 }}
		>
			<div class="p-4 border-b">
				<button
					class="p-2 rounded-md text-gray-500 hover:bg-gray-100 hover:text-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-500 float-right"
					aria-label="Close menu"
					on:click={closeMenu}
				>
					<svg class="w-5 h-5" viewBox="0 0 20 20" fill="currentColor">
						<path
							fill-rule="evenodd"
							d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z"
							clip-rule="evenodd"
						/>
					</svg>
				</button>
				<div class="mt-2">
					<img src={logo} alt={siteName} class="h-8 w-auto mx-auto" />
				</div>
			</div>
			<nav class="flex flex-col p-4 space-y-4">
				<a
					bind:this={firstMobileLink}
					href="/"
					class="text-gray-700 hover:text-blue-600 font-medium py-2 transition-colors"
					on:click={closeMenu}
				>
					Home
				</a>
				<a
					href="/shop"
					class="text-gray-700 hover:text-blue-600 font-medium py-2 transition-colors"
					on:click={closeMenu}
				>
					Shop
				</a>
				<a
					href="/about"
					class="text-gray-700 hover:text-blue-600 font-medium py-2 transition-colors"
					on:click={closeMenu}
				>
					About
				</a>
			</nav>
		</div>
	</div>
{/if}

<style lang="postcss">
	/* .badge {
		@apply absolute -top-1 -right-1 bg-blue-600 text-white text-xs font-medium px-1.5 min-w-[1.25rem] h-5 rounded-full flex items-center justify-center;
	} */
</style>
