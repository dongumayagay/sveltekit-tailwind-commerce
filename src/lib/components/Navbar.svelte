<script>
	import { showCart } from '$lib/stores';
	import Searchbar from './Searchbar.svelte';
	import { goto } from '$app/navigation';

	const logo = '/logo.webp';
	let showSearchbar = false;
	let innerWidth;

	function openCart() {
		if (innerWidth < 640) {
			goto('/cart');
		} else {
			$showCart = true;
		}
	}
</script>

<svelte:window bind:innerWidth />

<nav class="max-w-full py-1 bg-neutral-800 shadow-lg  text-neutral-300  sticky top-0 z-10">
	<div
		aria-label="Storefront"
		class="container flex justify-between mx-auto px-6 items-center space-x-5 lg:space-x-8"
	>
		<a sveltekit:prefetch href="/" class="pt-2 mr-auto">
			<img
				class="animate-sway origin-top h-14 lg:h-20 transition-all duration-500 ease-in-out"
				src={logo}
				alt="brand logo"
			/>
		</a>
		<a sveltekit:prefetch href={'/login'} aria-label="account"
			><svg
				xmlns="http://www.w3.org/2000/svg"
				class="h-6 lg:h-7 hover:text-yellow-500 transition-colors duration-500 stroke-[1.5] lg:stroke-2"
				fill="none"
				viewBox="0 0 24 24"
				stroke="currentColor"
			>
				<path
					stroke-linecap="round"
					stroke-linejoin="round"
					d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z"
				/>
			</svg></a
		>
		<!-- <button aria-label="search" on:click={() => (showSearchbar = true)}
			><svg
				xmlns="http://www.w3.org/2000/svg"
				class="h-6 lg:h-7 hover:text-yellow-500 transition-colors duration-500 stroke-[1.5] lg:stroke-2"
				fill="none"
				viewBox="0 0 24 24"
				stroke="currentColor"
			>
				<path
					stroke-linecap="round"
					stroke-linejoin="round"
					d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"
				/>
			</svg></button
		> -->
		<button aria-label="show cart" on:click={openCart}
			><svg
				xmlns="http://www.w3.org/2000/svg"
				class="h-6 lg:h-7 hover:text-yellow-500 transition-colors duration-500 stroke-[1.5] lg:stroke-2"
				fill="none"
				viewBox="0 0 24 24"
				stroke="currentColor"
			>
				<path
					stroke-linecap="round"
					stroke-linejoin="round"
					d="M3 3h2l.4 2M7 13h10l4-8H5.4M7 13L5.4 5M7 13l-2.293 2.293c-.63.63-.184 1.707.707 1.707H17m0 0a2 2 0 100 4 2 2 0 000-4zm-8 2a2 2 0 11-4 0 2 2 0 014 0z"
				/>
			</svg></button
		>
	</div>
</nav>
{#if showSearchbar}
	<Searchbar on:closeSearchbar={() => (showSearchbar = false)} />
{/if}
