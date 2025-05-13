<script>
	import { goto } from '$app/navigation';
	import { onMount } from 'svelte';

	let isMenuOpen = false;
	/**
	 * @type {HTMLDivElement}
	 */
	let menuRef;
	/**
	 * @type {HTMLDivElement}
	 */
	let hamburgerRef;

	// Toggle menu open/close
	const toggleMenu = () => {
		isMenuOpen = !isMenuOpen;
	};

	// Close menu when clicking a menu item
	const closeMenu = () => {
		isMenuOpen = false;
	};

	onMount(() => {
		// Handle clicks outside the menu to close it
		const handleClickOutside = (/** @type {{ target: any; }} */ event) => {
			if (
				isMenuOpen &&
				menuRef &&
				!menuRef.contains(event.target) &&
				hamburgerRef &&
				!hamburgerRef.contains(event.target)
			) {
				isMenuOpen = false;
			}
		};

		document.addEventListener('mousedown', handleClickOutside);

		// Clean up event listener on component destroy
		return () => {
			document.removeEventListener('mousedown', handleClickOutside);
		};
	});
</script>

<svelte:head>
	<!-- Add Iconify script -->
	<script src="https://code.iconify.design/iconify-icon/1.0.7/iconify-icon.min.js"></script>
</svelte:head>

<header class="w-full flex justify-center items-center">
	<section class="lg:w-5/6 w-full lg:rounded-xl border-b shadow-sm shadow-[#061C3D0D] bg-white px-4 md:px-10 h-16">
		<nav class="flex w-full items-center justify-between h-full">
			<!-- Logo -->
			<div>
				<a href="/" class="">
					<img width={150} height={150} src={'/logo.png'} alt="Logo" />
				</a>
			</div>

			<!-- Desktop Menu -->
			<div class="hidden md:block">
				<ul class="flex gap-4 lg:gap-8">
					<li on:click={()=>{goto("/#home")}} class="text-[#838E9E] hover:text-[#0B63E5] transition-colors duration-300 cursor-pointer">Home</li>
					<li on:click={()=>{goto("/#about")}} class="text-[#838E9E] hover:text-[#0B63E5] transition-colors duration-300 cursor-pointer whitespace-nowrap">About Us</li>
					<li on:click={()=>{goto("/#service")}} class="text-[#838E9E] hover:text-[#0B63E5] transition-colors duration-300 cursor-pointer">Services</li>
					<li on:click={()=>{goto("/#howWeWork")}} class="text-[#838E9E] hover:text-[#0B63E5] transition-colors duration-300 cursor-pointer">How We Work</li>
				</ul>
			</div>

			<!-- Contact Us Button - Desktop -->
			<div class="hidden md:block">
				<button on:click={()=>{goto('/#contact')}} class="bg-[#0B63E5] whitespace-nowrap py-2 lg:py-3 rounded-lg px-4 lg:px-6 text-sm lg:text-base font-semibold text-white hover:bg-[#0950c1] transition-colors duration-300">
					Contact Us
				</button>
			</div>

			<!-- Hamburger Menu - Mobile -->
			<div bind:this={hamburgerRef} class="md:hidden cursor-pointer" on:click={toggleMenu}>
				<iconify-icon
					icon={isMenuOpen ? "line-md:menu-to-close-transition" : "line-md:close-to-menu-transition"}
					width="28"
					height="28"
					class="text-[#0B63E5] transition-transform duration-700 ease-in-out"
				></iconify-icon>
			</div>
		</nav>
	</section>

	<!-- Mobile Menu Drawer - Only rendered when open -->
	{#if isMenuOpen}
		<div
			bind:this={menuRef}
			class="fixed top-16 right-0 w-64 bg-white shadow-lg h-[calc(100vh-4rem)] z-50 transition-all duration-700 slide-in-from-left-0 md:hidden"
		>
			<div class="p-6">
				<ul class="flex flex-col gap-6">
					<li on:click={()=>{goto("/#home"); closeMenu();}} class="text-[#838E9E] hover:text-[#0B63E5] transition-colors duration-300 cursor-pointer">
						<div class="flex items-center gap-3">
							<iconify-icon icon="mdi:home" width="20" height="20"></iconify-icon>
							<span>Home</span>
						</div>
					</li>
					<li on:click={()=>{goto("/#about"); closeMenu();}} class="text-[#838E9E] hover:text-[#0B63E5] transition-colors duration-300 cursor-pointer">
						<div class="flex items-center gap-3">
							<iconify-icon icon="mdi:information" width="20" height="20"></iconify-icon>
							<span>About Us</span>
						</div>
					</li>
					<li on:click={()=>{goto("/#service"); closeMenu();}} class="text-[#838E9E] hover:text-[#0B63E5] transition-colors duration-300 cursor-pointer">
						<div class="flex items-center gap-3">
							<iconify-icon icon="mdi:folder" width="20" height="20"></iconify-icon>
							<span>Services</span>
						</div>
					</li>
					<li on:click={()=>{goto("/#howWeWork"); closeMenu();}} class="text-[#838E9E] hover:text-[#0B63E5] transition-colors duration-300 cursor-pointer">
						<div class="flex items-center gap-3">
							<iconify-icon icon="mdi:cog" width="20" height="20"></iconify-icon>
							<span>How We Work</span>
						</div>
					</li>
				</ul>

				<!-- Contact Button - Mobile -->
				<div class="mt-8">
					<button on:click={()=>{goto('/#contact')}} class="bg-[#0B63E5] py-3 rounded-lg w-full text-base font-semibold text-white hover:bg-[#0950c1] transition-colors duration-300">
						Contact Us
					</button>
				</div>
			</div>
		</div>
	{/if}

	<!-- Overlay when menu is open -->
	<!-- {#if isMenuOpen}
		<div
			class="fixed inset-0 bg-black bg-opacity-50 z-40 md:hidden"
			on:click={closeMenu}
		></div>
	{/if} -->
</header>