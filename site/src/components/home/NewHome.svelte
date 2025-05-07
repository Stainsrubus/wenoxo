<script lang="ts">
    import { onMount } from 'svelte';

    let scrollContainer: HTMLElement;

    // Image sources for the carousel
    const imageSources = [
        '/pictures/p-1.jpg',
        '/pictures/p-2.jpg',
        '/pictures/p-3.jpg',
        '/pictures/p-4.jpg',
        '/pictures/p-6.jpg'
    ];

    // Text items for the text carousel
    const textItems = [
        'E-commerce',
        'Healthcare',
        'Finance',
        'Education',
        'Real Estate',
        'Logistics'
    ];

    onMount(() => {
        // Only apply scaling logic for desktop (lg and above)
        const isMobile = window.innerWidth < 1024; // Matches lg breakpoint
        if (!isMobile) {
            const updateImageScale = () => {
                const images = document.querySelectorAll('.right-images img');
                const viewportTop = scrollContainer.scrollTop;
                const viewportBottom = viewportTop + window.innerHeight;
                const viewportCenter = viewportTop + window.innerHeight / 2;

                let closestImage: HTMLImageElement | null = null;
                let minDistance = Infinity;

                images.forEach((img: HTMLImageElement) => {
                    const rect = img.getBoundingClientRect();
                    const imgTop = rect.top + scrollContainer.scrollTop;
                    const imgBottom = imgTop + rect.height;
                    const imgCenter = imgTop + rect.height / 2;

                    const isVisible = imgBottom > viewportTop && imgTop < viewportBottom;

                    if (isVisible) {
                        const distance = Math.abs(viewportCenter - imgCenter);
                        if (distance < minDistance) {
                            minDistance = distance;
                            closestImage = img;
                        }
                    }
                });

                images.forEach((img: HTMLImageElement) => {
                    if (img === closestImage) {
                        img.style.transform = 'scale(1.05)';
                        img.style.transition = 'transform 0.5s ease';
                    } else {
                        img.style.transform = 'scale(0.9)';
                        img.style.transition = 'transform 0.5s ease';
                    }
                });
            };

            scrollContainer.addEventListener('scroll', updateImageScale);
            updateImageScale();

            return () => {
                scrollContainer.removeEventListener('scroll', updateImageScale);
            };
        }

        const handleWheel = (event: WheelEvent) => {
            const { deltaY } = event;
            const scrollTop = scrollContainer.scrollTop;
            const scrollHeight = scrollContainer.scrollHeight;
            const clientHeight = scrollContainer.clientHeight;

            const atTop = scrollTop === 0 && deltaY < 0;
            const atBottom = scrollTop + clientHeight >= scrollHeight && deltaY > 0;

            if (atTop || atBottom) {
                return;
            } else {
                event.preventDefault();
                scrollContainer.scrollTop += deltaY;
            }
        };

        scrollContainer.addEventListener('wheel', handleWheel, { passive: false });

        return () => {
            scrollContainer.removeEventListener('wheel', handleWheel);
        };
    });
</script>

<svelte:head>
    <script src="https://cdn.tailwindcss.com"></script>
</svelte:head>

<div bind:this={scrollContainer} class="scroll-container lg:min-h-[100dvh] pt-10 lg:max-h-[100dvh] flex lg:overflow-y-auto">
    <div class="left-section lg:w-1/2 w-full text-gray-900 lg:px-12 px-5 flex flex-col lg:justify-between">
        <div>
            <h1 class="xl:text-6xl lg:text-5xl text-4xl font-bold my-10 text-center lg:text-left !leading-tight">Shape Your Ideas into Boundless Digital Experiences</h1>
            <p class="text-lg flex justify-center lg:justify-start mb-8">
                <button class="rounded-full border px-6 hover:scale-105 transition-all duration-300 py-1 hover:bg-zinc-800 hover:text-white cursor-pointer">Explore Now</button>
            </p>
        </div>

        <div class="flex flex-col gap-10 pb-48">
            <div class="flex flex-col gap-10 lg:hidden">
                <!-- Image carousel inside bordered wrapper -->
                <div class="wrapper lg:hidden">
                    {#each imageSources as src, index}
                        <div class="img-item" style="--i: {index + 1}">
                            <img src={src} alt={`Image ${index + 1}`} class="h-full w-full object-cover" />
                        </div>
                    {/each}
                    <!-- Duplicate for seamless looping -->
                    {#each imageSources as src, index}
                        <div class="img-item" style="--i: {index + imageSources.length + 1}">
                            <img src={src} alt={`Image ${index + 1}`} class="h-full w-full object-cover" />
                        </div>
                    {/each}
                </div>
            </div>

            <!-- Text carousel with CSS animation -->
            <div class="text-wrapper">
                {#each textItems as text, index}
                    <div class="text-item" style="--i: {index + 1}">
                        <p class="md:text-3xl text-xl font-semibold">{text}</p>
                    </div>
                {/each}
                <!-- Duplicate for seamless looping -->
                {#each textItems as text, index}
                    <div class="text-item" style="--i: {index + textItems.length + 1}">
                        <p class="md:text-3xl text-xl font-semibold">{text}</p>
                    </div>
                {/each}
            </div>
        </div>
    </div>

    <div class="right-section w-1/2 hidden lg:block">
        <div class="right-images flex flex-col gap-5 items-center p-5">
            <img src="/pictures/p-1.jpg" alt="Image 1" class="w-full max-h-[550px] object-contain mb-5 rounded-lg">
            <img src="/pictures/p-2.jpg" alt="Image 2" class="w-full max-h-[450px] object-contain mb-5 rounded-lg">
            <img src="/pictures/p-3.jpg" alt="Image 3" class="w-full max-h-[550px] object-contain mb-5 rounded-lg">
            <img src="/pictures/p-4.jpg" alt="Image 4" class="w-full max-h-[450px] object-contain mb-5 rounded-lg">
            <img src="/pictures/p-1.jpg" alt="Image 5" class="w-full max-h-[550px] object-contain mb-5 rounded-lg">
            <img src="/pictures/p-2.jpg" alt="Image 6" class="w-full max-h-[550px] object-contain mb-5 rounded-lg">
        </div>
    </div>
</div>

<style>
    :root {
        --duration: 8s; /* Duration for image animation */
        --totalitems: 5; /* Number of unique image items */
        --text-duration: 8s; /* Duration for text animation */
        --totaltextitems: 6; /* Number of unique text items */
        --image-width: 250px; /* Fixed width for images */
    }

    .wrapper {
        position: relative;
        display: flex;
        height: 10vw; /* Fixed height */
        width: 50vw; /* Fixed width */
        overflow: hidden;
        margin: 0 auto; /* Center the wrapper */
    }

    .img-item {
        position: absolute;
        height: inherit;
        width: var(--image-width); /* Fixed width */
        left: calc(-1 * var(--image-width) * var(--totalitems)); /* Start off-screen to the left */
        animation: scrollX var(--duration) linear infinite;
        animation-delay: calc(var(--duration) / var(--totalitems) * var(--i));
    }

    @keyframes scrollX {
        to {
            left: 100%; /* Move to off-screen right */
        }
    }

    .img-item img {
        height: 100%;
        width: 100%; /* Fill the fixed width */
        object-fit: cover; /* Ensure image fills the area */
    }

    .text-wrapper {
        position: relative;
        display: flex;
        height: 3rem; /* Fixed height for text */
        width: 50vw; /* Match wrapper width */
        overflow: hidden;
        margin: 0 auto; /* Center the text wrapper */
    }

    .text-item {
        position: absolute;
        height: inherit;
        width: 200px; /* Fixed width for text items */
        left: 100%; /* Start off-screen to the right */
        display: flex;
        align-items: center;
        animation: scrollText var(--text-duration) linear infinite;
        animation-delay: calc(var(--text-duration) / var(--totaltextitems) * (var(--totaltextitems) - var(--i)));
    }

    @keyframes scrollText {
        to {
            left: calc(-80px * var(--totaltextitems)); /* Move left by total width of text items */
        }
    }

    .scroll-container {
        scroll-snap-align: start;
    }

    .left-section {
        position: sticky;
        top: 0;
        height: 100vh;
        flex-shrink: 0;
    }

    .right-section {
        flex-grow: 1;
    }

    .right-images img {
        transform-origin: center;
    }

    @media (max-width: 1023px) {
        .scroll-container {
            min-height: auto;
            max-height: none;
            overflow-y: hidden;
            flex-direction: column;
        }

        .left-section {
            position: relative;
            height: auto;
            width: 100%;
        }

        .right-section {
            display: none;
        }

        .wrapper {
            width: 100vw; /* Full width on mobile */
            height: 150px; /* Fixed height for mobile */
        }

        .img-item {
            width: var(--image-width); /* Maintain fixed width */
        }

        @keyframes scrollX {
            to {
                left: 100%; 
            }
        }

        .text-wrapper {
            width: 90vw; /* Wider on mobile */
            height: 2.5rem; /* Slightly smaller for mobile */
        }
    }

    @media (min-width: 768px) and (max-width: 1023px) {
        .wrapper {
            width: 80vw; /* Slightly narrower for medium screens */
            height: 120px; /* Adjusted height for medium screens */
        }

        .text-wrapper {
            width: 80vw; /* Match wrapper width */
        }
    }
</style>