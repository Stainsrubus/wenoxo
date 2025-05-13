<script lang="ts">
	import { onMount } from 'svelte';
	import { browser } from '$app/environment';

	interface CardState {
		progress: number; // -1 to 1, based on scroll position
	}

	let cardStates = $state<CardState[]>(Array(4).fill({ progress: 0 }));
	let rafId: number | null = null;
	let lastScrollY = 0;

	onMount(() => {
		if (!browser) return; // Skip during SSR

		const cards = document.querySelectorAll('.card');
		let windowHeight = window.innerHeight || 600; // Fallback for SSR
		let windowWidth = window.innerWidth || 1024; // Fallback for SSR

		const updateCardStates = () => {
			const scrollY = window.scrollY;
			if (Math.abs(scrollY - lastScrollY) < 5) {
				rafId = null;
				return;
			}
			lastScrollY = scrollY;

			const isMobile = windowWidth < 768;
			const margin = isMobile ? windowHeight * 0.1 : windowHeight * 0.3; // Reduced margin for mobile
			const startPoint = isMobile ? windowHeight * 0.6 : windowHeight * 0.8; // Start earlier on mobile
			const endPoint = isMobile ? windowHeight * 0.2 : windowHeight * 0.1; // End sooner on mobile
			const animationRange = startPoint - endPoint;

			cardStates = Array.from(cards).map((card) => {
				const rect = card.getBoundingClientRect();
				const cardTop = rect.top;

				// Progress: -1 (above viewport), 0 (at start), 1 (at end)
				let progress = 1 - (cardTop - endPoint) / animationRange;
				progress = Math.max(-1, Math.min(1, progress));

				return { progress };
			});

			rafId = null;
		};

		const handleScroll = () => {
			if (!rafId) {
				rafId = requestAnimationFrame(updateCardStates);
			}
		};

		const handleResize = () => {
			windowHeight = window.innerHeight || 600;
			windowWidth = window.innerWidth || 1024;
			updateCardStates();
		};

		// Initial calculation
		setTimeout(updateCardStates, 100);

		window.addEventListener('scroll', handleScroll, { passive: true });
		window.addEventListener('resize', handleResize);

		return () => {
			window.removeEventListener('scroll', handleScroll);
			window.removeEventListener('resize', handleResize);
			if (rafId) cancelAnimationFrame(rafId);
		};
	});

	function easeOutCubic(x: number): number {
		return 1 - Math.pow(1 - x, 4); // Smoother easing
	}
</script>

<section class="grid grid-cols-1 lg:grid-cols-2 mt-10 px-4 sm:px-6 md:px-8 lg:px-16 lg:gap-x-6 gap-y-6 md:gap-y-8 lg:gap-y-10 lg:w-5/6 md:w-3/4 w-5/6 mx-auto">
	{#each cardStates as state, i}
		<div
			class="card w-full p-6 sm:p-8 md:p-12 lg:p-14 py-12 sm:py-16 md:py-20 lg:py-24 relative rounded-3xl shadow-lg bg-white will-change-transform"
			class:even={i % 2 === 0}
			class:odd={i % 2 !== 0}
			data-index={i}
			style="opacity: {easeOutCubic(Math.abs(state.progress))};
			       transform: {browser && window.innerWidth < 768
				? `translateX(${state.progress < 0 ? (1 + state.progress) * (i % 2 === 0 ? -8 : 8) : (1 - state.progress) * (i % 2 === 0 ? -8 : 8)}rem) scale(${0.9 + easeOutCubic(Math.abs(state.progress)) * 0.1})`
				: `translateX(${state.progress < 0 ? (1 + state.progress) * (i % 2 === 0 ? -24 : 24) : (1 - state.progress) * (i % 2 === 0 ? -24 : 24)}rem) rotate(${state.progress < 0 ? (1 + state.progress) * (i % 2 === 0 ? -10 : 10) : (1 - state.progress) * (i % 2 === 0 ? -10 : 10)}deg) scale(${0.9 + easeOutCubic(Math.abs(state.progress)) * 0.1})`};"
		>
			<div class="card-content relative z-10">
				<div>
					{#if i === 0}
						<p
							class="absolute w-fit p-3 sm:p-4 -top-12 sm:-top-16 md:-top-20 -right-2 sm:-right-3 md:-right-5 text-base sm:text-lg md:text-xl rounded-full bg-blue-100 border-gradient-to-r from-[#f8a5c2] to-[#f58b8c]"
						>
							Projects Delivered
						</p>
					{:else if i === 1}
						<p
							class="absolute w-fit p-3 sm:p-4 -top-12 sm:-top-16 md:-top-20 -right-2 sm:-right-3 md:-right-5 text-base sm:text-lg md:text-xl rounded-full bg-orange-100 border-gradient-to-r from-[#f8a5c2] to-[#f58b8c]"
						>
							Years Experience
						</p>
					{:else if i === 2}
						<p
							class="absolute w-fit p-3 sm:p-4 -top-12 sm:-top-16 md:-top-20 -right-2 sm:-right-3 md:-right-5 text-base sm:text-lg md:text-xl rounded-full bg-pink-100"
						>
							Industry Expertise
						</p>
					{:else}
						<p
							class="absolute w-fit p-3 sm:p-4 -top-12 sm:-top-16 md:-top-20 -right-2 sm:-right-3 md:-right-5 text-base sm:text-lg md:text-xl rounded-full bg-indigo-200 border-gradient-to-r from-[#f8a5c2] to-[#f58b8c]"
						>
							Satisfied Clients
						</p>
					{/if}
				</div>

				<p
					class="text-4xl sm:text-5xl md:text-6xl lg:text-7xl mt-8 sm:mt-10 font-bold text-gray-900"
				>
					{i === 0 ? '300' : i === 1 ? '12+' : i === 2 ? '10+' : '150'}
				</p>
				<p class="text-base sm:text-lg md:text-xl pt-8 sm:pt-10 md:pt-12 text-gray-600">
					{i === 0
						? 'Successful projects completed'
						: i === 1
							? 'Years of hands-on experience'
							: i === 2
								? 'Industries of Expertise'
								: 'Clients across diverse industries'}
				</p>
			</div>
		</div>
	{/each}
</section>

<style>
	section {
		scroll-behavior: smooth;
		perspective: 1000px;
	}

	.card {
		overflow: hidden;
		position: relative;
		backface-visibility: hidden;
		transition: opacity 0.2s ease-out, transform 0.2s ease-out; /* Faster transition on mobile */
	}

	.card.even {
		margin-bottom: 2.5rem;
	}

	.card.odd {
		margin-top: 2.5rem;
	}

	.card::before {
		content: '';
		position: absolute;
		top: -50%;
		left: -50%;
		width: 200%;
		height: 200%;
		background: conic-gradient(transparent, rgba(84, 84, 85, 0.4), transparent 30%);
		animation: rotate 4s linear infinite;
		pointer-events: none;
	}

	.card::after {
		content: '';
		position: absolute;
		inset: 1px;
		background: white;
		border-radius: 1.5rem;
		pointer-events: none;
	}

	@keyframes rotate {
		100% {
			transform: rotate(1turn);
		}
	}

	@media (max-width: 767px) {
		.card.even,
		.card.odd {
			margin-top: 0;
			margin-bottom: 0;
		}
	}
</style>