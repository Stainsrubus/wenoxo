<script lang="ts">
	import { writable } from 'svelte/store';

	// Track focus state for each input
	const focused = writable([false, false, false, false]); // [name, phone, email, message]

	// Track input values
	let name = '';
	let phone = '';
	let email = '';
	let message = '';

	// Functions to update focus state
	function handleFocus(index: number) {
		$focused[index] = true;
	}

	function handleBlur(index: number) {
		$focused[index] = false;
	}
</script>

<div class="px-4 py-12 bg-black text-white">
	<div class="flex flex-col md:flex-row justify-between items-center">
		<div class="text-center w-full md:w-1/3 md:text-left mb-8 md:mb-0">
			<h2 class="text-7xl leading-normal font-bold">Shall we get started?</h2>
		</div>
		<div class="w-full md:w-2/3">
			<form class="p-6 rounded-lg">
				<div class="mb-4">
					<label class="block text-gray-400 text-sm font-bold mb-2" for="service">
						I'm looking for help in ...
					</label>
					<div class="flex flex-wrap gap-2">
						<button type="button" class="bg-gray-900 hover:bg-gray-600 text-gray-400 py-2 px-4 rounded-full">
							App
						</button>
						<button type="button" class="bg-gray-900 hover:bg-gray-600 text-gray-400 py-2 px-4 rounded-full">
							Website
						</button>
						<button type="button" class="bg-gray-900 hover:bg-gray-600 text-gray-400 py-2 px-4 rounded-full">
							Logo
						</button>
						<button type="button" class="bg-gray-900 hover:bg-gray-600 text-gray-400 py-2 px-4 rounded-full">
							Branding
						</button>
						<button type="button" class="bg-gray-900 hover:bg-gray-600 text-gray-400 py-2 px-4 rounded-full">
							Design Consultation
						</button>
						<button type="button" class="bg-gray-900 hover:bg-gray-600 text-gray-400 py-2 px-4 rounded-full">
							Motion Graphics
						</button>
						<button type="button" class="bg-gray-900 hover:bg-gray-600 text-gray-400 py-2 px-4 rounded-full">
							Digital Marketing
						</button>
						<button type="button" class="bg-gray-900 hover:bg-gray-600 text-gray-400 py-2 px-4 rounded-full">
							Photography
						</button>
						<button type="button" class="bg-gray-900 hover:bg-gray-600 text-gray-400 py-2 px-4 rounded-full">
							Pitch Deck
						</button>
						<button type="button" class="bg-gray-900 hover:bg-gray-600 text-gray-400 py-2 px-4 rounded-full">
							Frontend Development
						</button>
						<button type="button" class="bg-gray-900 hover:bg-gray-600 text-gray-400 py-2 px-4 rounded-full">
							SEO
						</button>
						<button type="button" class="bg-gray-900 hover:bg-gray-600 text-gray-400 py-2 px-4 rounded-full">
							Other
						</button>
					</div>
				</div>
				<div class="mb-4">
					<label class="block text-gray-400 text-sm font-bold mb-2" for="budget">
						I'm willing to spend ...
					</label>
					<div class="flex flex-wrap gap-2">
						<button type="button" class="bg-gray-900 hover:bg-gray-600 text-gray-400 py-2 px-4 rounded-full">
							1Lac - 2Lac
						</button>
						<button type="button" class="bg-gray-900 hover:bg-gray-600 text-gray-400 py-2 px-4 rounded-full">
							2Lac - 3Lac
						</button>
						<button type="button" class="bg-gray-900 hover:bg-gray-600 text-gray-400 py-2 px-4 rounded-full">
							3Lac - 5Lac
						</button>
						<button type="button" class="bg-gray-900 hover:bg-gray-600 text-gray-400 py-2 px-4 rounded-full">
							5Lac
						</button>
					</div>
				</div>
				<div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-10">
					<!-- Name Input -->
					<div class="mb-4 relative">
						<input
							class="block w-full bg-transparent border-0 border-b-2 border-gray-800 appearance-none focus:outline-none focus:ring-0 focus:border-yellow-500 pt-4 pb-2 peer"
							id="name"
							type="text"
							bind:value={name}
							on:focus={() => handleFocus(0)}
							on:blur={() => handleBlur(0)}
							placeholder=" "
						/>
						<label
							for="name"
							class="absolute left-0 text-gray-400 text-sm font-bold transition-all duration-200 transform peer-placeholder-shown:top-4 peer-placeholder-shown:text-base peer-placeholder-shown:text-gray-400 peer-focus:-top-2 peer-focus:text-xs peer-focus:text-yellow-500 {$focused[0] || name ? '-top-6 text-xs text-yellow-500' : 'top-2 text-base'}"
						>
							My Name
						</label>
					</div>

					<!-- Phone Input -->
					<div class="mb-4 relative">
						<input
							class="block w-full bg-transparent border-0 border-b-2 border-gray-800 appearance-none focus:outline-none focus:ring-0 focus:border-yellow-500 pt-4 pb-2 peer"
							id="phone"
							type="text"
							bind:value={phone}
							on:focus={() => handleFocus(1)}
							on:blur={() => handleBlur(1)}
							placeholder=" "
						/>
						<label
							for="phone"
							class="absolute left-0 text-gray-400 text-sm font-bold transition-all duration-200 transform peer-placeholder-shown:top-4 peer-placeholder-shown:text-base peer-placeholder-shown:text-gray-400 peer-focus:-top-2 peer-focus:text-xs peer-focus:text-yellow-500 {$focused[1] || phone ? '-top-2 text-xs text-yellow-500' : 'top-4 text-base'}"
						>
							My Phone no.
						</label>
					</div>

					<!-- Email Input -->
					<div class=" relative">
						<input
							class="block w-full bg-transparent border-0 border-b-2 border-gray-800 appearance-none focus:outline-none focus:ring-0 focus:border-yellow-500 pt-4 pb-2 peer"
							id="email"
							type="email"
							bind:value={email}
							on:focus={() => handleFocus(2)}
							on:blur={() => handleBlur(2)}
							placeholder=" "
						/>
						<label
							for="email"
							class="absolute left-0 text-gray-400 text-sm font-bold transition-all duration-200 transform peer-placeholder-shown:top-4 peer-placeholder-shown:text-base peer-placeholder-shown:text-gray-400 peer-focus:-top-2 peer-focus:text-xs peer-focus:text-yellow-500 {$focused[2] || email ? '-top-2 text-xs text-yellow-500' : 'top-4 text-base'}"
						>
							My Email
						</label>
					</div>

					<!-- Message Textarea -->
					<div class=" relative">
						<textarea
							class="block  w-full bg-transparent border-0  border-b-2 py-0 border-gray-800 appearance-none focus:outline-none focus:ring-0 focus:border-yellow-500  peer"
							id="message"
							bind:value={message}
							on:focus={() => handleFocus(3)}
							on:blur={() => handleBlur(3)}
							placeholder=" "
						></textarea>
						<label
							for="message"
							class="absolute  left-0 text-gray-400 text-sm font-bold transition-all duration-200 transform peer-placeholder-shown:top-4 peer-placeholder-shown:text-base peer-placeholder-shown:text-gray-400 peer-focus:-top-3 peer-focus:text-xs peer-focus:text-yellow-500 {$focused[3] || message ? '-top-4 text-xs text-yellow-500' : 'top-6 text-base'}"
						>
							Tell us more...
						</label>
					</div>
				</div>
				<div class="flex items-center justify-between">
					<button class="bg-yellow-500 hover:bg-yellow-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline" type="button">
						Send
					</button>
				</div>
			</form>
		</div>
	</div>
</div>