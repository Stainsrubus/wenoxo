<script lang="ts">
	import { onMount } from 'svelte';

	interface CardState {
		progress: number; // 0 to 1, based on scroll position
		visible: boolean;
	}

	let cardStates = $state<CardState[]>(Array(4).fill({ progress: 0, visible: false }));
	let rafId: number | null = null;

	onMount(() => {
		const cards = document.querySelectorAll('.card');
		let windowHeight = window.innerHeight;
		
		// Calculate trigger points relative to viewport
		const updateCardStates = () => {
			// Use more appropriate margins based on screen size
			const margin = window.innerWidth < 768 ? windowHeight * 0.15 : windowHeight * 0.3;
			
			// The point where card starts transitioning in (further down)
			const startPoint = windowHeight + margin;
			// The point where card is fully visible (higher up)
			const endPoint = windowHeight * 0.2;
			// Total range for the animation
			const animationRange = startPoint - endPoint;

			cardStates = Array.from(cards).map((card, index) => {
				const rect = card.getBoundingClientRect();
				const cardTop = rect.top;
				
				// Calculate progress: 0 when card just enters viewport, 1 when it's at target position
				// Ensure smooth transition through the entire range
				let progress = 1 - ((cardTop - endPoint) *2/ animationRange);
				
				// Make sure progress stays in 0-1 range
				progress = Math.max(0, Math.min(1, progress));
				
				// Apply slight adjustment to make initial movement more noticeable
				if (progress > 0 && progress < 0.2) {
					progress = progress * 0.7; // Slow down initial movement to make translation more visible
				}
				
				// Determine if card should be visible (slightly before entering viewport)
				const visible = cardTop < (windowHeight + 200);
				
				return { progress, visible };
			});
			
			rafId = null;
		};

		const handleScroll = () => {
			// Use requestAnimationFrame for smoother updates
			if (!rafId) {
				rafId = requestAnimationFrame(updateCardStates);
			}
		};

		const handleResize = () => {
			windowHeight = window.innerHeight;
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

	// Helper function for enhanced smooth easing
	function easeOutCubic(x: number): number {
		// Using a more pronounced easing curve
		return 1 - Math.pow(1 - x, 8);
	}
</script>

<section class="grid grid-cols-1 lg:grid-cols-2 mt-10 px-4 sm:px-6 md:px-8 lg:px-16 lg:gap-x-6 gap-y-6 md:gap-y-8 lg:gap-y-10 lg:w-5/6 md:w-3/4 w-5/6 mx-auto">
	{#each cardStates as state, i}
		<div
			class="card w-full p-6 sm:p-8 md:p-12 lg:p-14 py-12 sm:py-16 md:py-20 lg:py-24 relative rounded-3xl shadow-lg bg-white will-change-transform"
			class:even={i % 2 === 0}
			class:odd={i % 2 !== 0}
			data-index={i}
			style="opacity: {state.visible ? easeOutCubic(state.progress) : 0};
			       transform: {window.innerWidth < 768
				? `scale(${0.9 + (easeOutCubic(state.progress) * 0.1)}) translateY(${(1 - easeOutCubic(state.progress)) * 3}rem)`
				: i % 2 === 0
					? `translateX(${(1 - easeOutCubic(state.progress)) * -16}rem) rotate(${(1 - easeOutCubic(state.progress)) * -8}deg) scale(${0.9 + (easeOutCubic(state.progress) * 0.1)})`
					: `translateX(${(1 - easeOutCubic(state.progress)) * 16}rem) rotate(${(1 - easeOutCubic(state.progress)) * 8}deg) scale(${0.9 + (easeOutCubic(state.progress) * 0.1)})`};"
		>
			<div class="card-content relative z-10">
				<div>
					{#if i === 0}
						<p
							class="absolute w-fit p-3 sm:p-4 -top-12 sm:-top-16 md:-top-20 -right-2 sm:-right-3 md:-right-5 text-base sm:text-lg md:text-xl rounded-full bg-gray-100 border-2 border-gradient-to-r from-[#f8a5c2] to-[#f58b8c]"
						>
							Business Driven
						</p>
					{:else if i === 1}
						<p
							class="absolute w-fit -top-12 sm:-top-16 md:-top-20 -left-2 sm:-left-3 md:-left-5 text-base sm:text-lg md:text-xl flex items-center"
						>
							<img src="/assets/home/icons.svg" alt="Delivered" class="h-16 sm:h-20 md:h-24" />
						</p>
					{:else if i === 2}
						<p
							class="absolute w-fit -top-12 sm:-top-16 md:-top-20 -left-2 sm:-left-3 md:-left-5 text-base sm:text-lg md:text-xl flex items-center"
						>
							<img src="/assets/home/icons.svg" alt="Delivered" class="h-16 sm:h-20 md:h-24" />
						</p>
					{:else}
						<p
							class="absolute w-fit p-3 sm:p-4 -top-12 sm:-top-16 md:-top-20 -right-2 sm:-right-3 md:-right-5 text-base sm:text-lg md:text-xl rounded-full bg-gray-100"
						>
							Experience
						</p>
					{/if}
				</div>

				<p
					class="text-4xl sm:text-5xl md:text-6xl lg:text-7xl mt-8 sm:mt-10 font-bold text-gray-900"
				>
					{i === 0 ? '$300m' : i === 1 ? '200+' : i === 2 ? '150+' : '10+ Years'}
				</p>
				<p class="text-base sm:text-lg md:text-xl pt-8 sm:pt-10 md:pt-12 text-gray-600">
					{i === 0
						? 'In funding clients raised'
						: i === 1
							? 'Completed worldwide'
							: i === 2
								? 'Satisfied customers'
								: 'Software development'}
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
		transition: opacity 0.3s ease-out, transform 0.3s ease-out;
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