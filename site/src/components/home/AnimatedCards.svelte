<script lang="ts">
    import { onMount } from 'svelte';

    let visibleCards = Array(4).fill(false);
    let prevScrollPos = 0;
    let ticking = false; // For scroll performance optimization

    onMount(() => {
        prevScrollPos = window.scrollY;

        const handleScroll = () => {
            if (!ticking) {
                window.requestAnimationFrame(() => {
                    prevScrollPos = window.scrollY;
                    ticking = false;
                });
                ticking = true;
            }
        };

        window.addEventListener('scroll', handleScroll, { passive: true });

        // Responsive observer with dynamic rootMargin based on screen size
        const getRootMargin = () => {
            const isSmallScreen = window.innerWidth < 768; // md breakpoint
            return isSmallScreen ? '-10% 0px -10% 0px' : '-25% 0px -25% 0px';
        };

        const observer = new IntersectionObserver(
            (entries) => {
                entries.forEach((entry) => {
                    const index = parseInt(entry.target.getAttribute('data-index') || '-1');
                    if (index !== -1) {
                        visibleCards = [
                            ...visibleCards.slice(0, index),
                            entry.isIntersecting,
                            ...visibleCards.slice(index + 1)
                        ];
                    }
                });
            },
            {
                threshold: [0.1,0.2,0.3,0.4,0.5,0.6,0.7,0.8,0.9,1], // Adjusted for better detection
                rootMargin: getRootMargin()
            }
        );

        document.querySelectorAll('.card').forEach((card) => observer.observe(card));

        // Initial visibility check with relaxed conditions for smaller screens
        setTimeout(() => {
            document.querySelectorAll('.card').forEach((card, index) => {
                const rect = card.getBoundingClientRect();
                const windowHeight = window.innerHeight || document.documentElement.clientHeight;
                const isVisible = rect.top >= -windowHeight * 0.5 && rect.bottom <= windowHeight * 1.5;

                if (isVisible) {
                    visibleCards = [
                        ...visibleCards.slice(0, index),
                        true,
                        ...visibleCards.slice(index + 1)
                    ];
                }
            });
        }, 100);

        // Update rootMargin on resize
        const handleResize = () => {
            observer.disconnect();
            document.querySelectorAll('.card').forEach((card) => observer.observe(card));
            observer.takeRecords(); // Force re-evaluation
        };

        window.addEventListener('resize', handleResize);

        return () => {
            observer.disconnect();
            window.removeEventListener('scroll', handleScroll);
            window.removeEventListener('resize', handleResize);
        };
    });
</script>

<section class="grid grid-cols-1 lg:grid-cols-2 mt-10 px-4 sm:px-6 md:px-8 lg:px-16 lg:gap-x-6 gap-y-6 md:gap-y-8 lg:gap-y-10 lg:w-5/6 md:w-3/4  w-5/6 mx-auto">
    {#each Array(4) as _, i}
        <div
            class="card w-full p-6 sm:p-8 md:p-12 lg:p-14 py-12 sm:py-16 md:py-20 lg:py-24 relative rounded-3xl shadow-lg bg-white hover:scale-105 transition-all duration-300 ease-out will-change-transform"
            class:visible={visibleCards[i]}
            class:left={i % 2 === 0}
            class:right={i % 2 !== 0}
            class:even={i % 2 === 0}
            class:odd={i % 2 !== 0}
            data-index={i}
        >
            <div class="card-content relative z-10">
                <div>
                    {#if i === 0}
                        <p class="absolute w-fit p-3 sm:p-4 -top-12 sm:-top-16 md:-top-20 -right-2 sm:-right-3 md:-right-5 text-base sm:text-lg md:text-xl rounded-full bg-gray-100 border-2 border-gradient-to-r from-[#f8a5c2] to-[#f58b8c]">
                            Business Driven
                        </p>
                    {:else if i === 1}
                        <p class="absolute w-fit -top-12 sm:-top-16 md:-top-20 -left-2 sm:-left-3 md:-left-5 text-base sm:text-lg md:text-xl flex items-center">
                            <img src="/assets/home/icons.svg" alt="Delivered" class="h-16 sm:h-20 md:h-24" />
                        </p>
                    {:else if i === 2}
                        <p class="absolute w-fit -top-12 sm:-top-16 md:-top-20 -left-2 sm:-left-3 md:-left-5 text-base sm:text-lg md:text-xl flex items-center">
                            <img src="/assets/home/icons.svg" alt="Delivered" class="h-16 sm:h-20 md:h-24" />
                        </p>
                    {:else}
                        <p class="absolute w-fit p-3 sm:p-4 -top-12 sm:-top-16 md:-top-20 -right-2 sm:-right-3 md:-right-5 text-base sm:text-lg md:text-xl rounded-full bg-gray-100">
                            Experience
                        </p>
                    {/if}
                </div>

                <p class="text-4xl sm:text-5xl md:text-6xl lg:text-7xl mt-8 sm:mt-10 font-bold text-gray-900">
                    {i === 0 ? "$300m" : i === 1 ? "200+" : i === 2 ? "150+" : "10+ Years"}
                </p>
                <p class="text-base sm:text-lg md:text-xl pt-8 sm:pt-10 md:pt-12 text-gray-600">
                    {i === 0
                        ? "In funding clients raised"
                        : i === 1
                          ? "Completed worldwide"
                          : i === 2
                            ? "Satisfied customers"
                            : "Software development"}
                </p>
            </div>
        </div>
    {/each}
</section>

<style>
    section {
        scroll-behavior: smooth;
        perspective: 1000px; /* Adding perspective for more natural 3D animations */
    }

    .card {
        overflow: hidden;
        position: relative;
        opacity: 0;
        transform: scale(0.9);
        transition: opacity 0.8s ease-out, transform 1.2s cubic-bezier(0.19, 1, 0.22, 1);
        backface-visibility: hidden; /* Prevents flickering during animations */
    }

    .card.left {
        transform: translateX(-8rem) rotate(-10deg) scale(0.95); /* Reduced offset */
    }

    .card.right {
        transform: translateX(8rem) rotate(10deg) scale(0.95); /* Reduced offset */
    }

    .card.even {
        margin-bottom: 2.5rem;
    }

    .card.odd {
        margin-top: 2.5rem;
    }

    .card.visible {
        opacity: 1;
        transform: translateX(0) rotate(0deg) scale(1);
    }

    .card::before {
        content: '';
        position: absolute;
        top: -50%;
        left: -50%;
        width: 100%;
        height: 100%;
        background: conic-gradient(transparent, rgba(84, 84, 85, 0.4), transparent 30%);
        animation: rotate 4s linear infinite;
        pointer-events: none;
    }

    .card::after {
        content: '';
        position: absolute;
        inset: 1px;
        background: white;
        border-radius: 1.5rem; /* Matches rounded-3xl */
        pointer-events: none;
    }

    @keyframes rotate {
        100% {
            transform: rotate(1turn);
        }
    }

    @media (max-width: 767px) {
        .card {
            transform: scale(0.95);
        }

        .card.left,
        .card.right {
            transform: translateX(0) rotate(0deg) scale(0.95);
        }

        .card.even,
        .card.odd {
            margin-top: 0;
            margin-bottom: 0; /* Handled by grid gap */
        }

        .card.visible {
            opacity: 1;
            transform: scale(1);
        }
    }
</style>