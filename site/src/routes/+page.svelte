<script>
	import Service from '../components/home/Service.svelte';
	import Header from '../components/shared/Header.svelte';
	import Footer from '../components/shared/Footer.svelte';
	import Performance from '../components/home/performance.svelte';
	import glassmorphism from "$lib/images/glassmorphism.png";
	import icons from "$lib/images/Group 1948754638.png";

	import Home from '../components/home/Home.svelte';
	import Milestones from '../components/home/Milestones.svelte';
	import HowWeWork from '../components/home/How_we_work.svelte';
	import Cursor from '../components/shared/Cursor.svelte';
	import Projects from '../components/home/Projects.svelte';
	import { onMount } from 'svelte';
	import NewHome from '../components/home/NewHome.svelte';
  
	let isHomeSectionInView = true;
  
	// This will only run on the client side
	onMount(() => {
	  if (typeof IntersectionObserver !== 'undefined') {
		const observer = new IntersectionObserver((entries) => {
		  entries.forEach(entry => {
			if (entry.isIntersecting) {
			  isHomeSectionInView = true;
			} else {
			  isHomeSectionInView = false;
			}
		  });
		}, {
		  threshold: 0.5 // Adjust the threshold as needed
		});
  
		const homeSection = document.getElementById('home-section');
		if (homeSection) {
		  observer.observe(homeSection);
		}
	  }
	});
  </script>
  
  <style>
	.header {
	  transition: padding-top 0.5s ease, top 0.3s ease;
	}
  
	.header.pt-10 {
	  padding-top: 2.5rem; /* Equivalent to pt-10 in Tailwind CSS */
	}
  
	.header.top-0 {
	  top: 0;
	}
  </style>
  
  <main class="relative">
	<img src={glassmorphism} class="absolute" alt="">
	<!-- <img src={icons} class="absolute" alt=""> -->
	<Cursor />
	<div class="sticky top-0 z-50 backdrop-blur-sm ">
	  <div class={`header ${isHomeSectionInView ? 'lg:pt-8' : 'pt-0'} pt-0`}>
		<Header />
	  </div>
	</div>
	<div id="home-section" class="z-20">
	  <!-- <Home /> -->
	  <NewHome />
	</div>
	<Milestones />

	<!-- <Performance /> -->
	<!-- <Projects /> -->
	<HowWeWork />
	<Service />

	<Footer />
  </main>
  