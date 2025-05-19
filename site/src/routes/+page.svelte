<script>
  import Service from '../components/home/Service.svelte';
  import Header from '../components/shared/Header.svelte';
  import Footer from '../components/shared/Footer.svelte';
  import Performance from '../components/home/performance.svelte';
  import glassmorphism from '$lib/images/glassmorphism.png';
  import icons from '$lib/images/Group 1948754638.png';
  import Milestones from '../components/home/Milestones.svelte';
  import HowWeWork from '../components/home/How_we_work.svelte';
  import Cursor from '../components/shared/Cursor.svelte';
  import { onMount } from 'svelte';
  import NewHome from '../components/home/NewHome.svelte';
  import Contact from '@/components/home/contact.svelte';
  import Demo from '@/components/home/demo.svelte';

  let isHomeSectionInView = true;
  let isContactSectionInView = false;
  let isFooterInView = false;

  onMount(() => {
    if (typeof IntersectionObserver !== 'undefined') {
      const observer = new IntersectionObserver(
        (entries) => {
          entries.forEach((entry) => {
            if (entry.target.id === 'home') {
              isHomeSectionInView = entry.isIntersecting;
            } else if (entry.target.id === 'contact') {
              isContactSectionInView = entry.isIntersecting;
            } else if (entry.target.id === 'footer') {
              isFooterInView = entry.isIntersecting;
            }
          });

          const header = document.querySelector('.header');
          if (header) {
            if (isContactSectionInView || isFooterInView) {
              header.classList.add('hidden');
            } else {
              header.classList.remove('hidden');
            }
          }
        },
        { threshold: 0.1 } // Lowered threshold to detect when even 10% of the section is in view
      );

      const homeSection = document.getElementById('home');
      const contactSection = document.getElementById('contact');
      const footer = document.getElementById('footer');

      [homeSection, contactSection, footer].forEach((section) => {
        if (section) observer.observe(section);
      });

      // Debugging: Log state changes
      console.log('Initial isHomeSectionInView:', isHomeSectionInView);
      return () => {
        [homeSection, contactSection, footer].forEach((section) => {
          if (section) observer.unobserve(section);
        });
      };
    }
  });
</script>

<style>
  .header {
    transition: opacity 0.9s ease, transform 0.9s ease; /* Reduced transition duration for smoother effect */
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 50;
    backdrop-filter: blur(10px);
  }

  .header.hidden {
    opacity: 0;
    transform: translateY(-100%);
    pointer-events: none;
  }
</style>

<main class="relative">
  <div class="hidden lg:block">
    <Cursor />
  </div>

  <div class={`header ${isHomeSectionInView ? 'lg:pt-8' : 'pt-0'} transition-all duration-900`}>
    <Header />
  </div>

  <div id="home" class="z-30">
    <Demo />
  </div>

  <div id="about" class="mt-0">
    <Milestones />
  </div>

  <div id="service" class="mt-0">
    <Service />
  </div>

  <div class="mt-0">
    <HowWeWork />
  </div>

  <div id="howWeWork" class="mt-0">
    <!-- How we work content -->
  </div>

  <div class="mt-0">
    <Service />
  </div>

  <div id="contact" class="mt-0">
    <Contact />
  </div>

  <div id="footer" class="mt-0">
    <Footer />
  </div>
</main>