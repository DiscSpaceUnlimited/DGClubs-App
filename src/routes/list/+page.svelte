
<script>
	/** @type {{ data: import('./$types').PageData }} */
	let { data } = $props();
	import { onMount } from 'svelte';

	// State for filters and search
	let searchQuery = $state('');
	let selectedState = $state('all');
	let selectedDistance = $state('all');
	let sortBy = $state('name'); // name, distance, memberCount

	// Mock club data (in real app, this would come from an API)
	let clubs = $state([
		{
			id: 1,
			name: 'River Valley Disc Golf Club',
			location: {
				city: 'Springfield',
				state: 'ST',
				coordinates: { lat: 44.9778, lng: -93.2650 }
			},
			memberCount: 180,
			courseCount: 2,
			established: 2009,
			pdgaAffiliated: true,
			distance: 0, // Will be calculated based on user's location
			website: 'https://rivervalleydgc.org',
			featured: true,
			description: 'A vibrant disc golf community managing two scenic courses along the river valley.',
			amenities: ['Tournaments', 'Leagues', 'Pro Shop', 'Lessons']
		},
		{
			id: 2,
			name: 'Mountain View Disc Golf Association',
			location: {
				city: 'Boulder',
				state: 'CO',
				coordinates: { lat: 40.0150, lng: -105.2705 }
			},
			memberCount: 245,
			courseCount: 3,
			established: 2005,
			pdgaAffiliated: true,
			distance: 0,
			website: 'https://mvdga.org',
			featured: true,
			description: 'Home to three championship-level mountain courses with breathtaking views.',
			amenities: ['Tournaments', 'Leagues', 'Training Center']
		},
		// Add more mock clubs here
	]);

	// Filter options
	const states = [
		{ value: 'all', label: 'All States' },
		{ value: 'ST', label: 'Springfield' },
		{ value: 'CO', label: 'Colorado' },
		// Add more states
	];

	const distanceOptions = [
		{ value: 'all', label: 'Any Distance' },
		{ value: '25', label: 'Within 25 miles' },
		{ value: '50', label: 'Within 50 miles' },
		{ value: '100', label: 'Within 100 miles' }
	];

	const sortOptions = [
		{ value: 'name', label: 'Name' },
		{ value: 'distance', label: 'Distance' },
		{ value: 'memberCount', label: 'Member Count' }
	];

	// Filtered and sorted clubs
	let filteredClubs = $derived(clubs)
		filteredClubs.filter(club => {
			const matchesSearch = club.name.toLowerCase().includes(searchQuery.toLowerCase()) ||
				club.location.city.toLowerCase().includes(searchQuery.toLowerCase());
			const matchesState = selectedState === 'all' || club.location.state === selectedState;
			const matchesDistance = selectedDistance === 'all' || 
				(club.distance <= parseInt(selectedDistance));
			
			return matchesSearch && matchesState && matchesDistance;
		})
		.sort((a, b) => {
			switch (sortBy) {
				case 'name':
					return a.name.localeCompare(b.name);
				case 'distance':
					return a.distance - b.distance;
				case 'memberCount':
					return b.memberCount - a.memberCount;
				default:
					return 0;
			}
		});

	// Function to calculate distances (mock implementation)
	function calculateDistances(userLat, userLng) {
		clubs = clubs.map(club => ({
			...club,
			distance: Math.round(
				Math.random() * 100 // Mock distance calculation
			)
		}));
	}

	onMount(() => {
		// In a real app, we would get the user's location here
		calculateDistances(44.9778, -93.2650);
	});
</script>

<div class="min-h-screen bg-gray-50 py-12">
	<div class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
		<!-- Header Section -->
		<div class="mb-8 text-center">
			<h1 class="text-3xl font-bold text-gray-900 sm:text-4xl">Find a Disc Golf Club</h1>
			<p class="mt-4 text-lg text-gray-600">
				Connect with disc golf communities in your area and start playing today
			</p>
		</div>

		<!-- Search and Filters -->
		<div class="mb-8 rounded-lg bg-white p-6 shadow-sm">
			<div class="grid gap-4 md:grid-cols-2 lg:grid-cols-4">
				<!-- Search Input -->
				<div>
					<label for="search" class="block text-sm font-medium text-gray-700">Search</label>
					<input
						type="text"
						id="search"
						bind:value={searchQuery}
						placeholder="Search by name or city"
						class="mt-1 block w-full rounded-md border border-gray-300 px-3 py-2 text-gray-900 placeholder-gray-500 focus:border-blue-500 focus:outline-none"
					/>
				</div>

				<!-- State Filter -->
				<div>
					<label for="state" class="block text-sm font-medium text-gray-700">State</label>
					<select
						id="state"
						bind:value={selectedState}
						class="mt-1 block w-full rounded-md border border-gray-300 px-3 py-2 text-gray-900 focus:border-blue-500 focus:outline-none"
					>
						{#each states as state}
							<option value={state.value}>{state.label}</option>
						{/each}
					</select>
				</div>

				<!-- Distance Filter -->
				<div>
					<label for="distance" class="block text-sm font-medium text-gray-700">Distance</label>
					<select
						id="distance"
						bind:value={selectedDistance}
						class="mt-1 block w-full rounded-md border border-gray-300 px-3 py-2 text-gray-900 focus:border-blue-500 focus:outline-none"
					>
						{#each distanceOptions as option}
							<option value={option.value}>{option.label}</option>
						{/each}
					</select>
				</div>

				<!-- Sort By -->
				<div>
					<label for="sort" class="block text-sm font-medium text-gray-700">Sort By</label>
					<select
						id="sort"
						bind:value={sortBy}
						class="mt-1 block w-full rounded-md border border-gray-300 px-3 py-2 text-gray-900 focus:border-blue-500 focus:outline-none"
					>
						{#each sortOptions as option}
							<option value={option.value}>{option.label}</option>
						{/each}
					</select>
				</div>
			</div>
		</div>

		<!-- Results Count -->
		<div class="mb-4 text-sm text-gray-600">
			Found {filteredClubs.length} clubs
		</div>

		<!-- Club List -->
		<div class="space-y-6">
			{#each filteredClubs as club (club.id)}
				<div class="overflow-hidden rounded-lg bg-white shadow-sm transition-shadow hover:shadow-md">
					<div class="p-6">
						<div class="flex flex-col justify-between gap-4 sm:flex-row sm:items-start">
							<div>
								<div class="flex items-center">
									<h2 class="text-xl font-bold text-gray-900">
										<a href="/clubs/{club.id}" class="hover:text-blue-600">
											{club.name}
										</a>
									</h2>
									{#if club.pdgaAffiliated}
										<span class="ml-2 rounded-full bg-blue-100 px-2 py-1 text-xs font-medium text-blue-800">
											PDGA Affiliated
										</span>
									{/if}
									{#if club.featured}
										<span class="ml-2 rounded-full bg-yellow-100 px-2 py-1 text-xs font-medium text-yellow-800">
											Featured
										</span>
									{/if}
								</div>
								<p class="mt-1 text-gray-600">
									{club.location.city}, {club.location.state}
									{#if club.distance}
										<span class="text-gray-500">• {club.distance} miles away</span>
									{/if}
								</p>
								<p class="mt-2 text-gray-600">{club.description}</p>
								<div class="mt-4 flex flex-wrap gap-2">
									{#each club.amenities as amenity}
										<span class="rounded-full bg-gray-100 px-3 py-1 text-sm text-gray-600">
											{amenity}
										</span>
									{/each}
								</div>
							</div>
							<div class="text-right">
								<div class="space-y-1 text-sm text-gray-600">
									<p>{club.memberCount} members</p>
									<p>{club.courseCount} courses</p>
									<p>Est. {club.established}</p>
								</div>
								<div class="mt-4 flex flex-col gap-2">
									<a
										href="/clubs/{club.id}"
										class="rounded-lg bg-blue-600 px-4 py-2 text-center text-white hover:bg-blue-700"
									>
										View Club
									</a>
									<a
										href={club.website}
										target="_blank"
										rel="noopener noreferrer"
										class="rounded-lg border border-gray-300 px-4 py-2 text-center text-gray-600 hover:bg-gray-50"
									>
										Visit Website
									</a>
								</div>
							</div>
						</div>
					</div>
				</div>
			{/each}
		</div>

		<!-- No Results -->
		{#if filteredClubs.length === 0}
			<div class="rounded-lg bg-white p-8 text-center">
				<p class="text-gray-600">No clubs found matching your criteria.</p>
				<button
					class="mt-4 text-blue-600 hover:text-blue-700"
					on:click={() => {
						searchQuery = '';
						selectedState = 'all';
						selectedDistance = 'all';
					}}
				>
					Clear all filters
				</button>
			</div>
		{/if}

		<!-- Start a Club CTA -->
		<div class="mt-12 rounded-lg bg-blue-50 p-8 text-center">
			<h2 class="text-2xl font-bold text-gray-900">Don't see a club in your area?</h2>
			<p class="mt-2 text-gray-600">Start your own club and grow the disc golf community!</p>
			<a
				href="/start-club"
				class="mt-4 inline-block rounded-lg bg-blue-600 px-6 py-3 text-white hover:bg-blue-700"
			>
				Learn How to Start a Club
			</a>
		</div>
	</div>
</div>