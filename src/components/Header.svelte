<script lang="ts">
	import menuIcon from '../lib/assets/images/menu-icon.png';
	import logo from '../lib/assets/images/logo.jpg';

	// Simple, reusable header props
	import { tick, onDestroy } from 'svelte';
	// guard DOM access during SSR
	const isBrowser = typeof document !== 'undefined' && typeof window !== 'undefined';
	import { fade, fly } from 'svelte/transition';

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

<header class="  w-full bg-white container">
	<div class=" max-w-7xl mx-auto flex items-center gap-3 px-4 py-3">
		<!-- hamburger: visible on small screens -->
		<button
			class="block sm:hidden p-2 rounded-md text-gray-700 hover:bg-gray-100"
			aria-label="Toggle menu"
			aria-controls="mobile-menu"
			aria-expanded={menuOpen}
			on:click={toggleMenu}
		>
			<img src={menuIcon} alt="menu_icon" class="w-6 h-6" />
		</button>

		<!-- left nav: expanded width on larger screens -->
		<nav class="hidden sm:flex flex-1 sm:flex-2 gap-4 items-center" aria-label="Main navigation">
			<a href="/" class="text-gray-800 font-semibold px-3 py-1 rounded-md hover:bg-gray-100">Home</a
			>
			<a href="/shop" class="text-gray-800 font-semibold px-3 py-1 rounded-md hover:bg-gray-100"
				>Collection</a
			>
			<a href="/about" class="text-gray-800 font-semibold px-3 py-1 rounded-md hover:bg-gray-100"
				>New</a
			>
		</nav>

		<!-- middle logo -->
		<div class="flex-0 text-center">
			<a href="/" class="text-lg font-bold text-gray-900 px-3"> <img src={logo} alt="logo" class="w-8 h-8"> </a>
		</div>

		<!-- right icons -->
		<div class="flex items-center gap-2 justify-end flex-1">
			<button class="p-2 rounded-md text-gray-700 hover:bg-gray-100" aria-label="Favorites">
				<svg
					width="20"
					height="20"
					viewBox="0 0 24 24"
					fill="none"
					xmlns="http://www.w3.org/2000/svg"
					aria-hidden="true"
				>
					<path
						d="M12 21s-7.5-4.873-9.172-7.072A5.5 5.5 0 0112 3.5a5.5 5.5 0 019.172 10.428C19.5 16.127 12 21 12 21z"
						stroke="currentColor"
						stroke-width="1.2"
						stroke-linecap="round"
						stroke-linejoin="round"
					/>
				</svg>
				{#if favCount > 0}
					<span
						class="absolute -top-1 -right-1 bg-red-500 text-white text-xs px-2 py-0.5 rounded-full"
						>{favCount}</span
					>
				{/if}
			</button>

			<button class="icon-btn" aria-label="Cart">
				<svg
					width="20"
					height="20"
					viewBox="0 0 24 24"
					fill="none"
					xmlns="http://www.w3.org/2000/svg"
					aria-hidden="true"
				>
					<path
						d="M6 6h15l-1.5 9h-12L4 2H2"
						stroke="currentColor"
						stroke-width="1.2"
						stroke-linecap="round"
						stroke-linejoin="round"
					/>
					<circle cx="10" cy="20" r="1" fill="currentColor" />
					<circle cx="18" cy="20" r="1" fill="currentColor" />
				</svg>
				{#if cartCount > 0}
					<span class="badge">{cartCount}</span>
				{/if}
			</button>

			<a
				class="p-2 rounded-md text-gray-700 hover:bg-gray-100"
				href="/profile"
				aria-label="Profile"
			>
				<svg
					width="20"
					height="20"
					viewBox="0 0 24 24"
					fill="none"
					xmlns="http://www.w3.org/2000/svg"
					aria-hidden="true"
				>
					<path
						d="M20 21v-2a4 4 0 00-4-4H8a4 4 0 00-4 4v2"
						stroke="currentColor"
						stroke-width="1.2"
						stroke-linecap="round"
						stroke-linejoin="round"
					/>
					<circle
						cx="12"
						cy="7"
						r="4"
						stroke="currentColor"
						stroke-width="1.2"
						stroke-linecap="round"
						stroke-linejoin="round"
					/>
				</svg>
			</a>
		</div>
	</div>
</header>

{#if menuOpen}
	<button
		class="fixed inset-0 bg-black/40 z-40"
		on:click={closeMenu}
		aria-label="Close mobile menu"
		transition:fade
	></button>
	<div
		id="mobile-menu"
		class="fixed top-0 left-0 h-screen w-[min(320px,80vw)] bg-white z-50 p-5 shadow-lg flex flex-col gap-3"
		role="dialog"
		aria-label="Mobile menu"
		aria-modal="true"
		transition:fly={{ x: -320, duration: 220 }}
	>
		<button class="self-end text-2xl leading-none" aria-label="Close menu" on:click={closeMenu}
			>×</button
		>
		<nav class="flex flex-col gap-3 mt-2">
			<a
				bind:this={firstMobileLink}
				href="/"
				class="text-gray-800 font-semibold"
				on:click={closeMenu}>Home</a
			>
			<a href="/shop" class="text-gray-800 font-semibold" on:click={closeMenu}>Shop</a>
			<a href="/about" class="text-gray-800 font-semibold" on:click={closeMenu}>About</a>
		</nav>
	</div>
{/if}

<!-- styles moved to Tailwind utility classes -->
