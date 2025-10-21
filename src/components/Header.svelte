<script lang="ts">
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

<header class="wrapper">
	<div class="container">
		<button
			class="hamburger"
			aria-label="Toggle menu"
			aria-controls="mobile-menu"
			aria-expanded={menuOpen}
			on:click={toggleMenu}
		>
			<svg
				width="24"
				height="24"
				viewBox="0 0 24 24"
				fill="none"
				xmlns="http://www.w3.org/2000/svg"
				aria-hidden="true"
			>
				<path
					d="M3 6h18M3 12h18M3 18h18"
					stroke="currentColor"
					stroke-width="1.6"
					stroke-linecap="round"
					stroke-linejoin="round"
				/>
			</svg>
		</button>

		<nav class="left" aria-label="Main navigation">
			<a href="/" class="nav-link">Home</a>
			<a href="/shop" class="nav-link">Shop</a>
			<a href="/about" class="nav-link">About</a>
		</nav>

		<div class="middle">
			<a href="/" class="logo">{siteName}</a>
		</div>

		<div class="right" aria-hidden="false">
			<button class="icon-btn" aria-label="Favorites">
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
					<span class="badge">{favCount}</span>
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

			<a class="icon-btn" href="/profile" aria-label="Profile">
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
	<button class="overlay" on:click={closeMenu} aria-label="Close mobile menu" transition:fade
	></button>
	<div
		id="mobile-menu"
		class="mobile-menu"
		role="dialog"
		aria-label="Mobile menu"
		aria-modal="true"
		transition:fly={{ x: -320, duration: 220 }}
	>
		<button class="close-btn" aria-label="Close menu" on:click={closeMenu}> × </button>
		<nav class="mobile-nav">
			<a bind:this={firstMobileLink} href="/" class="nav-link" on:click={closeMenu}>Home</a>
			<a href="/shop" class="nav-link" on:click={closeMenu}>Shop</a>
			<a href="/about" class="nav-link" on:click={closeMenu}>About</a>
		</nav>
	</div>
{/if}

<style>
	.wrapper {
		border: 2px solid #e6e6e6;
		padding: 12px 16px;
		border-radius: 8px;
		max-width: 1100px;
		margin: 16px auto;
		background-color: #fff;
	}

	.container {
		display: flex;
		align-items: center;
		gap: 12px;
	}

	/* Left: nav links */
	.left {
		display: flex;
		gap: 12px;
		align-items: center;
		flex: 1 1 0;
	}

	.nav-link {
		color: #333;
		text-decoration: none;
		font-weight: 600;
		padding: 6px 8px;
		border-radius: 6px;
	}

	.nav-link:hover,
	.nav-link:focus {
		background: #f3f4f6;
		outline: none;
	}

	/* Middle: centered logo */
	.middle {
		flex: 0 0 auto;
		text-align: center;
	}

	.logo {
		font-size: 1.15rem;
		font-weight: 700;
		color: #111827;
		text-decoration: none;
		padding: 6px 12px;
		border-radius: 6px;
	}

	/* Right: icons */
	.right {
		display: flex;
		gap: 8px;
		align-items: center;
		justify-content: flex-end;
		flex: 1 1 0;
	}

	.icon-btn {
		display: inline-flex;
		align-items: center;
		justify-content: center;
		width: 40px;
		height: 40px;
		background: transparent;
		border: none;
		border-radius: 8px;
		color: #374151;
		cursor: pointer;
		position: relative;
		text-decoration: none;
	}

	.icon-btn:hover,
	.icon-btn:focus {
		background: #f3f4f6;
		outline: none;
	}

	.icon-btn svg {
		display: block;
	}

	.badge {
		position: absolute;
		top: -6px;
		right: -6px;
		background: #ef4444;
		color: white;
		font-size: 0.7rem;
		padding: 2px 6px;
		border-radius: 999px;
		line-height: 1;
	}

	/* Small screens: stack and reduce spacing */
	@media (max-width: 640px) {
		.container {
			gap: 8px;
		}

		/* show hamburger, hide full nav on small screens */
		.hamburger {
			display: inline-flex;
			align-items: center;
			justify-content: center;
			width: 40px;
			height: 40px;
			background: transparent;
			border: none;
			border-radius: 8px;
			cursor: pointer;
			color: #374151;
		}

		.left {
			display: none; /* collapse nav to keep header compact; mobile menu used instead */
		}

		.middle {
			margin: 0 auto;
		}
	}

	/* Mobile menu panel and overlay */
	.overlay {
		position: fixed;
		inset: 0;
		background: rgba(0, 0, 0, 0.4);
		z-index: 40;
	}

	.mobile-menu {
		position: fixed;
		top: 0;
		left: 0;
		height: 100vh;
		width: min(320px, 80vw);
		background: white;
		z-index: 50;
		padding: 20px;
		box-shadow: 2px 0 12px rgba(0, 0, 0, 0.08);
		display: flex;
		flex-direction: column;
		gap: 12px;
	}

	.mobile-nav {
		display: flex;
		flex-direction: column;
		gap: 8px;
		margin-top: 8px;
	}

	.close-btn {
		align-self: flex-end;
		background: transparent;
		border: none;
		font-size: 1.5rem;
		line-height: 1;
		cursor: pointer;
	}

	/* end of styles */
</style>
