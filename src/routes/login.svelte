<script context="module">
	export const prerender = true;
	import { browser } from '$app/env';
	import { auth } from '$lib/firebase';
	import { session } from '$lib/stores';
	import { get } from 'svelte/store';

	export async function load() {
		if (browser) {
			if (get(session)) return { status: 300, redirect: '/account' };
		}
		return {};
	}
</script>

<script>
	import { navigating } from '$app/stores';
	import { formatErrorCode } from '$lib/utils';
	import { signInWithEmailAndPassword } from 'firebase/auth';
	import { goto } from '$app/navigation';

	$: if ($session) goto('/account');
	let errorMessage;

	async function submitHandler(event) {
		const formData = new FormData(event.target);
		let data = Object.fromEntries(formData);
		try {
			await signInWithEmailAndPassword(auth, data.email, data.password);
		} catch (error) {
			console.log(error.code);
			errorMessage = formatErrorCode(error.code);
		}
	}
</script>

<svelte:head><title>Login | Divisorry Price</title></svelte:head>
{#if !$session}
	<div class="flex-1 flex justify-center items-center">
		<main
			class="flex flex-col bg-white border border-neutral-300/75 shadow-xl w-full sm:w-[450px] py-12 px-6 rounded-xl"
		>
			<h1 class="uppercase font-bold tracking-widest text-3xl pb-4 text-center">
				Login into your Account
			</h1>
			<form class="flex flex-col" on:submit|preventDefault={submitHandler}>
				<label class="pb-2">
					<span class="inline-block text-sm pl-4"> Email </span>
					<input
						on:focus={() => (errorMessage = null)}
						required
						name="email"
						class="w-full rounded-full px-4 text-lg"
						type="email"
						placeholder="Email"
					/>
				</label>
				<label class="pb-6">
					<span class="inline-block text-sm pl-4"> Password </span>
					<input
						on:focus={() => (errorMessage = null)}
						required
						name="password"
						class="w-full rounded-full px-4 text-lg"
						type="password"
						placeholder="Password"
					/>
				</label>
				{#if errorMessage}
					<p class="mb-4 text-center text-red-500">{errorMessage}</p>
				{/if}
				<button
					class="grid grid-cols-3 items-center  bg-neutral-900 rounded-full px-4 py-3 capitalize tracking-widest text-lg font-medium text-white"
					type="submit"
				>
					<span class="col-span-1" />
					<span class="col-span-1 "> login </span>
					<span class="col-span-1 flex justify-center">
						{#if $navigating}
							<svg
								class="animate-spin -ml-1 mr-3 h-5 w-5 text-white"
								xmlns="http://www.w3.org/2000/svg"
								fill="none"
								viewBox="0 0 24 24"
							>
								<circle
									class="opacity-25"
									cx="12"
									cy="12"
									r="10"
									stroke="currentColor"
									stroke-width="4"
								/>
								<path
									class="opacity-75"
									fill="currentColor"
									d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"
								/>
							</svg>
						{/if}
					</span>
				</button>
				<a
					sveltekit:prefetch
					href="/signup"
					class="mx-auto text-center transition pt-4 text-lg text-neutral-600 w-fit border-transparent border-b hover:border-black"
				>
					Don't have an account? Create one
				</a>
			</form>
		</main>
	</div>
{/if}
