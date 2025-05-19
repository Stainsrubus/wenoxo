<script>
  import Service from '../components/home/Service.svelte';
  import Header from '../components/shared/Header.svelte';
  import Footer from '../components/shared/Footer.svelte';
  import Performance from '../components/home/performance.svelte';
  import glassmorphism from "$lib/images/glassmorphism.png";
  import icons from "$lib/images/Group 1948754638.png";
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
      const observer = new IntersectionObserver((entries) => {
        entries.forEach(entry => {
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
      }, { threshold: 0.5 });

      const homeSection = document.getElementById('home');
      const contactSection = document.getElementById('contact');
      const footer = document.getElementById('footer');

      [homeSection, contactSection, footer].forEach(section => {
        if (section) observer.observe(section);
      });
    }
  });
</script>

<style>
  .header {
    transition: opacity 3s ease, transform 3s ease;
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
  <!-- Fixed background image -->
  <!-- <img src={glassmorphism} class={`absolute w-full h-screen object-cover -z-10`} alt="" loading="lazy"> -->
  
  
  <div class="hidden lg:block">
    <Cursor />
  </div>
  
  <div class="header">
    <Header />
  </div>
  
  <div id="home" class="z-30 ">
    <Demo />
  </div>
  
  <div id="about">
    <Milestones />
  </div>
  
  <div id="service">
    <!-- Service content -->
  </div>
  
  <HowWeWork />
  
  <div id="howWeWork">
    <!-- How we work content -->
  </div>
  
  <Service />
  
  <div id="contact">
    <Contact />
  </div>
  
  <div id="footer">
    <Footer />
  </div>
</main>