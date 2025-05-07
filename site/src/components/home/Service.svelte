<script>
	import { ArrowRight } from 'lucide-svelte';
	import volchek from '$lib/video/volchek-color.mp4';
	import ball from '$lib/video/ball-color.mp4';
	import spring from '$lib/video/pruzina-color.mp4';
	import clock from '$lib/video/time-color.mp4';
	import Chart from '../shared/Chart.svelte';
</script>

<main class="mt-10 px-5 md:px-10 lg:px-20">
	<div class="flex flex-col md:flex-row md:items-end md:justify-between pb-10">
		<h1 class="text-left font-sans text-4xl md:text-5xl lg:text-6xl font-semibold text-[#3F3F3F]">
			We Strive to Innovate
		</h1>

		<!-- Button with hover effect -->
		<p
			class="group relative flex cursor-pointer items-center gap-2 overflow-hidden rounded-full bg-gray-100 px-6 py-2 text-lg mt-5 md:mt-0 w-fit"
		>
			<span
				class="group-hover:animate-fallIn relative z-10 transition-all duration-500 group-hover:text-white"
			>
				Become a client
			</span>
			<span
				class="group-hover:animate-fallIn relative z-10 transition-all duration-500 group-hover:text-white"
			>
				<ArrowRight size={20} />
			</span>

			<!-- Flow effect -->
			<span
				class="absolute inset-0 origin-right scale-x-0 bg-black transition-transform duration-300 ease-out group-hover:scale-x-100"
			></span>
		</p>
	</div>
	<section class="">
		<div class="">
			<p class="text-xl md:text-2xl lg:text-3xl font-light text-neutral-400">
				<span class="text-black">Solid Strategy</span> aligned with business needs and robust data analysis
				are fundamental ingredients to extract actionable insights
			</p>

			<div class="flex flex-col lg:flex-row">
				<div class="mt-10 md:mt-20 w-full  lg:w-1/2">
					<p class="text-lg md:text-xl lg:text-2xl font-semibold text-neutral-400">Some Numbers About Us</p>
					<div class="grid grid-cols-1 md:grid-cols-2 justify-center items-center gap-6 mt-5 md:mt-10">
						{#each [
							{ number: "+300", text: "We have successfully completed a total of 300+ projects", video: volchek },
							{ number: "100K", text: "We’ve gathered dozens of reviews from the clients and + 100k reviews from their users", video: spring },
							{ number: "+10", text: "Years of experience", video: clock },
							{ number: "+280", text: "Team members all over the world", video: ball }
						] as item}
							<div class="rounded-[50px] relative max-h-[250px] max-w-[400px] bg-[#f2f0f0] p-10 pb-20 overflow-hidden">
								<!-- Higher z-index for text -->
								<p class="text-4xl md:text-5xl lg:text-6xl font-semibold relative z-10">{item.number}</p>
								<p class="text-neutral-500 w-2/3 text-sm md:text-base relative z-10 mt-2">{item.text}</p>

								<!-- Lower z-index for the video -->
								<div class="absolute -bottom-8 -right-6 z-0">
									<video class="w-40 rounded-[50px]" playsinline autoplay loop muted>
										<source src={item.video} type="video/mp4">
									</video>
								</div>
							</div>
						{/each}
					</div>
				</div>
				<div class="w-full lg:w-1/2 mt-10 md:mt-20 flex justify-center items-center">
					<Chart />
				</div>
			</div>
		</div>

		<hr class="mt-10 md:mt-20" />
	</section>
</main>

<style>
	@keyframes fallIn {
		0% {
			transform: translateY(-10px);
			opacity: 0;
		}
		100% {
			transform: translateY(0);
			opacity: 1;
		}
	}

	.group-hover\:animate-fallIn {
		animation: fallIn 0.3s ease-out forwards;
	}
</style>
