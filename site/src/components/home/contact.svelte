<script lang="ts">
	import { writable } from 'svelte/store';

	// Track focus state for each input
	const focused = writable([false, false, false, false, false]); // [name, phone, email, message, sector]

	// Track selected services
	const selectedServices = writable<string[]>([]);

	// Track selected business sectors
	const selectedSectors = writable<string[]>([]);

	// Track input values and feedback
	let name = '';
	let phone = '';
	let email = '';
	let message = '';
	let phoneError = '';
	let formMessage = '';
	let formMessageType = ''; // 'success' or 'error'

	// List of available services
	const services = [
		'Mobile App',
		'Website',
		'Cloud Application',
		'ERP Software',
		'CMS Development',
		'UI/UX Design'
	];

	// List of business sectors
	const sectors = [
		'E-commerce',
		'Education',
		'Finance',
		'Health',
		'Real Estate',
		'Logistics',
		'Other'
	];

	// Toggle selection of a service
	function toggleService(service: string) {
		selectedServices.update((current) => {
			if (current.includes(service)) {
				return current.filter((s) => s !== service);
			} else {
				return [...current, service];
			}
		});
	}

	// Toggle selection of a sector
	function toggleSector(sector: string) {
		selectedSectors.update((current) => {
			if (current.includes(sector)) {
				return current.filter((s) => s !== sector);
			} else {
				return [...current, sector];
			}
		});
	}

	// Functions to update focus state
	function handleFocus(index: number) {
		$focused[index] = true;
	}

	function handleBlur(index: number) {
		$focused[index] = false;
	}

	// Validate phone number (allow only digits)
	function validatePhone(event: Event) {
		const input = event.target as HTMLInputElement;
		const numericValue = input.value.replace(/[^0-9]/g, '');
		phone = numericValue;
		phoneError = numericValue.length > 0 ? '' : 'Please enter a valid phone number (digits only)';
	}

	// Handle form submission
	async function handleSubmit(event: Event) {
		event.preventDefault();
		const form = event.target as HTMLFormElement;

		// Validate phone number before submission
		if (!phone || phoneError) {
			formMessage = 'Please provide a valid phone number.';
			formMessageType = 'error';
			return;
		}

		const payload = {
			service_id: 'service_70nfi2e',
			template_id: 'template_hxj6cst',
			user_id: '4neAUe9l0jy_Tyrus',
			template_params: {
				name: name,
				phone: phone,
				email: email,
				message: message,
				services: $selectedServices.join(', '),
				sectors: $selectedSectors.join(', ')
			}
		};

		try {
			// Send email using EmailJS
			const response = await fetch('https://api.emailjs.com/api/v1.0/email/send', {
				method: 'POST',
				headers: {
					'Content-Type': 'application/json',
				},
				body: JSON.stringify(payload),
			});

			if (response.status === 200) {
				formMessage = 'Your message has been sent successfully!';
				formMessageType = 'success';
				form.reset();
				name = phone = email = message = '';
				selectedServices.set([]);
				selectedSectors.set([]);
			} else {
				formMessage = 'Failed to send message. Please try again.';
				formMessageType = 'error';
			}
		} catch (error) {
			console.error('Submission error:', error);
			formMessage = 'An error occurred. Please try again later.';
			formMessageType = 'error';
		}
	}
</script>

<div class="px-4 pt-12 bg-black text-white">
	<div class="flex flex-col md:flex-row justify-between items-center">
		<div class="text-center w-full md:w-1/3 md:text-left mb-8 md:mb-0">
			<h2 class="lg:text-7xl md:text-6xl text-5xl leading-normal font-bold">Shall we get started?</h2>
		</div>
		<div class="w-full md:w-2/3">
			<form class="p-6 rounded-lg" on:submit={handleSubmit}>
				{#if formMessage}
					<p class="mb-4 text-sm {formMessageType === 'success' ? 'text-green-500' : 'text-red-500'}">
						{formMessage}
					</p>
				{/if}
				<div class="mb-10">
					<label class="block text-gray-400 text-sm font-bold mb-4" for="sector">
						Choose your business sector
					</label>
					<div class="flex flex-wrap gap-2">
						{#each sectors as sector}
							<button
								type="button"
								class="{$selectedSectors.includes(sector) ? 'bg-gray-500 text-white' : 'bg-gray-900'} hover:bg-gray-600 text-gray-400 hover:text-white py-2 px-4 rounded-full transition-colors"
								on:click={() => toggleSector(sector)}
							>
								{sector}
							</button>
						{/each}
					</div>
				</div>
				<div class="mb-10">
					<label class="block text-gray-400 text-sm font-bold mb-4" for="service">
						Select the digital service that matches your requirements
					</label>
					<div class="flex flex-wrap gap-2">
						{#each services as service}
							<button
								type="button"
								class="{$selectedServices.includes(service) ? 'bg-gray-500 text-white' : 'bg-gray-900'} hover:bg-gray-600 text-gray-400 hover:text-white py-2 px-4 rounded-full transition-colors"
								on:click={() => toggleService(service)}
							>
								{service}
							</button>
						{/each}
					</div>
				</div>
				<!-- Hidden input to store selected services -->
				<input
					type="hidden"
					name="services"
					value={$selectedServices.join(',')}
				/>
				<!-- Hidden input to store selected sectors -->
				<input
					type="hidden"
					name="sectors"
					value={$selectedSectors.join(',')}
				/>
				<div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-10">
					<!-- Name Input -->
					<div class="mb-4 relative">
						<input
							class="block w-full bg-transparent border-0 border-b-2 border-gray-800 appearance-none focus:outline-none focus:ring-0 focus:border-blue-500 pt-4 pb-2 peer"
							id="name"
							type="text"
							name="name"
							bind:value={name}
							on:focus={() => handleFocus(0)}
							on:blur={() => handleBlur(0)}
							placeholder=" "
						/>
						<label
							for="name"
							class="absolute left-0 text-gray-400 text-sm font-bold transition-all duration-200 transform peer-placeholder-shown:top-4 peer-placeholder-shown:text-base peer-placeholder-shown:text-gray-400 peer-focus:-top-2 peer-focus:text-xs peer-focus:text-blue-500 {$focused[0] || name ? '-top-6 text-xs text-blue-500' : 'top-2 text-base'}"
						>
							My Name
						</label>
					</div>

					<!-- Phone Input -->
					<div class="mb-4 relative">
						<input
							class="block w-full bg-transparent border-0 border-b-2 border-gray-800 appearance-none focus:outline-none focus:ring-0 focus:border-blue-500 pt-4 pb-2 peer {phoneError ? 'border-red-500' : ''}"
							id="phone"
							type="text"
							name="phone"
							bind:value={phone}
							on:input={validatePhone}
							on:focus={() => handleFocus(1)}
							on:blur={() => handleBlur(1)}
							placeholder=" "
						/>
						<label
							for="phone"
							class="absolute left-0 text-gray-400 text-sm font-bold transition-all duration-200 transform peer-placeholder-shown:top-4 peer-placeholder-shown:text-base peer-placeholder-shown:text-gray-400 peer-focus:-top-2 peer-focus:text-xs peer-focus:text-blue-500 {$focused[1] || phone ? '-top-2 text-xs text-blue-500' : 'top-4 text-base'}"
						>
							My Mobile no.
						</label>
						{#if phoneError}
							<p class="text-red-500 text-xs mt-1">{phoneError}</p>
						{/if}
					</div>

					<!-- Email Input -->
					<div class="relative">
						<input
							class="block w-full bg-transparent border-0 border-b-2 border-gray-800 appearance-none focus:outline-none focus:ring-0 focus:border-blue-500 pt-4 pb-2 peer"
							id="email"
							type="email"
							name="email"
							bind:value={email}
							on:focus={() => handleFocus(2)}
							on:blur={() => handleBlur(2)}
							placeholder=" "
						/>
						<label
							for="email"
							class="absolute left-0 text-gray-400 text-sm font-bold transition-all duration-200 transform peer-placeholder-shown:top-4 peer-placeholder-shown:text-base peer-placeholder-shown:text-gray-400 peer-focus:-top-2 peer-focus:text-xs peer-focus:text-blue-500 {$focused[2] || email ? '-top-2 text-xs text-blue-500' : 'top-4 text-base'}"
						>
							My Email
						</label>
					</div>

					<!-- Message Textarea -->
					<div class="relative">
						<textarea
							class="block w-full bg-transparent border-0 border-b-2 py-0 border-gray-800 appearance-none focus:outline-none focus:ring-0 focus:border-blue-500 peer"
							id="message"
							name="message"
							bind:value={message}
							on:focus={() => handleFocus(3)}
							on:blur={() => handleBlur(3)}
							placeholder=" "
						></textarea>
						<label
							for="message"
							class="absolute left-0 text-gray-400 text-sm font-bold transition-all duration-200 transform peer-placeholder-shown:top-4 peer-placeholder-shown:text-base peer-placeholder-shown:text-gray-400 peer-focus:-top-3 peer-focus:text-xs peer-focus:text-blue-500 {$focused[3] || message ? '-top-4 text-xs text-blue-500' : 'top-6 text-base'}"
						>
							Tell us more...
						</label>
					</div>
				</div>
				<div class="flex items-center justify-between">
					<button
						class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline"
						type="submit"
					>
						Send
					</button>
				</div>
			</form>
		</div>
	</div>
</div>
