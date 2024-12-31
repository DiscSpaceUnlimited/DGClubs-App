<script>
	import '../../app.css';
	let { children } = $props();
	import { onMount } from 'svelte';
	import { slide, fade } from 'svelte/transition';
	import { quartOut } from 'svelte/easing';

	// State
	let isMobileMenuOpen = $state(false);
	let isScrolled = $state(false);

	// Navigation data
	const navLinks = [
		{
			label: 'Find Clubs',
			href: '/clubs',
			subitems: [
				{ label: 'Browse All', href: '/clubs' },
				{ label: 'Near Me', href: '/clubs/near-me' },
				{ label: 'Featured Clubs', href: '/clubs/featured' }
			]
		},
		{
			label: 'Resources',
			href: '/resources',
			subitems: [
				{ label: 'Start a Club', href: '/resources/start-club' },
				{ label: 'Club Management', href: '/resources/management' },
				{ label: 'Best Practices', href: '/resources/best-practices' }
			]
		},
		{
			label: 'Events',
			href: '/events',
			subitems: [
				{ label: 'Tournaments', href: '/events/tournaments' },
				{ label: 'Leagues', href: '/events/leagues' },
				{ label: 'Club Meetups', href: '/events/meetups' }
			]
		},
		{
			label: 'News',
			href: '/news'
		}
	];

	// Handle scroll events
	onMount(() => {
		const handleScroll = () => {
			isScrolled = window.scrollY > 10;
		};

		window.addEventListener('scroll', handleScroll);
		return () => window.removeEventListener('scroll', handleScroll);
	});

	// Toggle mobile menu
	function toggleMobileMenu() {
		isMobileMenuOpen = !isMobileMenuOpen;
	}
</script>

<nav
	class="sticky top-0 z-50 bg-white transition-shadow duration-200 {isScrolled ? 'shadow-lg' : ''}"
>
	<div class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
		<div class="flex h-16 justify-between">
			<!-- Logo and Brand -->
			<div class="flex items-center">
				<a href="/" class="flex items-center">
					<span class="text-2xl font-bold text-blue-600">DGClubs</span>
					<span class="ml-2 text-sm text-gray-600">Club Directory</span>
				</a>
			</div>

			<!-- Desktop Navigation -->
			<div class="hidden items-center space-x-4 md:flex">
				{#each navLinks as link}
					{#if link.subitems}
						<div class="group relative">
							<button class="flex items-center px-3 py-2 text-gray-600 hover:text-blue-600">
								{link.label}
								<svg class="ml-1 h-4 w-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
									<path
										stroke-linecap="round"
										stroke-linejoin="round"
										stroke-width="2"
										d="M19 9l-7 7-7-7"
									/>
								</svg>
							</button>
							<!-- Dropdown Menu -->
							<div class="absolute left-0 mt-2 w-48">
								<div
									class="invisible rounded-md bg-white shadow-lg transition-all duration-200 group-hover:visible"
									in:fade={{ duration: 200 }}
									out:fade={{ duration: 150 }}
								>
									{#each link.subitems as subitem}
										<a
											href={subitem.href}
											class="block px-4 py-2 text-sm text-gray-700 hover:bg-blue-50"
										>
											{subitem.label}
										</a>
									{/each}
								</div>
							</div>
						</div>
					{:else}
						<a href={link.href} class="px-3 py-2 text-gray-600 hover:text-blue-600">{link.label}</a>
					{/if}
				{/each}
			</div>

			<!-- Right Side - Auth Buttons -->
			<div class="hidden items-center space-x-4 md:flex">
				<a href="/club-login" class="text-gray-600 hover:text-blue-600">Club Login</a>
				<a
					href="/list-your-club"
					class="rounded-lg bg-blue-600 px-4 py-2 text-white hover:bg-blue-700"
				>
					List Your Club
				</a>
			</div>

			<!-- Mobile Menu Button -->
			<div class="flex items-center md:hidden">
				<button
					on:click={toggleMobileMenu}
					class="rounded-md p-2 hover:bg-gray-100 focus:outline-none"
					aria-label="Toggle mobile menu"
				>
					<svg class="h-6 w-6 text-gray-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
						<path
							stroke-linecap="round"
							stroke-linejoin="round"
							stroke-width="2"
							d="M4 6h16M4 12h16M4 18h16"
						/>
					</svg>
				</button>
			</div>
		</div>
	</div>

	<!-- Mobile Menu -->
	{#if isMobileMenuOpen}
		<div class="md:hidden" transition:slide={{ duration: 200, easing: quartOut }}>
			<div class="space-y-1 px-4 pb-3 pt-2">
				{#each navLinks as link}
					<div class="border-b border-gray-200 py-2">
						{#if link.subitems}
							<!-- Mobile Dropdown -->
							<button
								class="flex w-full items-center justify-between px-3 py-2 text-base font-medium text-gray-600"
							>
								<span>{link.label}</span>
								<svg class="h-4 w-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
									<path
										stroke-linecap="round"
										stroke-linejoin="round"
										stroke-width="2"
										d="M19 9l-7 7-7-7"
									/>
								</svg>
							</button>
							<div class="ml-4 mt-1 space-y-1" transition:slide={{ duration: 150 }}>
								{#each link.subitems as subitem}
									<a
										href={subitem.href}
										class="block px-3 py-2 text-sm text-gray-500 hover:text-blue-600"
									>
										{subitem.label}
									</a>
								{/each}
							</div>
						{:else}
							<a
								href={link.href}
								class="block px-3 py-2 text-base font-medium text-gray-600 hover:text-blue-600"
							>
								{link.label}
							</a>
						{/if}
					</div>
				{/each}

				<!-- Mobile Auth Buttons -->
				<div class="border-t border-gray-200 pt-4" transition:slide={{ duration: 150, delay: 50 }}>
					<a
						href="/club-login"
						class="block px-3 py-2 text-base font-medium text-gray-600 hover:text-blue-600"
					>
						Club Login
					</a>
					<a
						href="/list-your-club"
						class="mt-2 block rounded-lg bg-blue-600 px-3 py-2 text-center text-base font-medium text-white hover:bg-blue-700"
					>
						List Your Club
					</a>
				</div>
			</div>
		</div>
	{/if}
</nav>

<main class="min-h-screen bg-gray-50">
	{@render children()}
</main>

<!-- Footer -->
<footer class="bg-gray-800 text-white">
	<div class="mx-auto max-w-7xl px-4 py-12 sm:px-6 lg:px-8">
		<div class="grid gap-8 md:grid-cols-4">
			<!-- Platform Info -->
			<div>
				<h3 class="mb-4 text-2xl font-bold text-blue-400">DGClubs</h3>
				<p class="text-gray-400">Connecting disc golfers with local clubs worldwide</p>
				<div class="mt-4 flex space-x-4">
					<a href="#facebook" class="text-gray-400 hover:text-white">
						<span class="sr-only">Facebook</span>
						<svg class="h-6 w-6" fill="currentColor" viewBox="0 0 24 24">
							<path
								d="M22 12c0-5.523-4.477-10-10-10S2 6.477 2 12c0 4.991 3.657 9.128 8.438 9.878v-6.987h-2.54V12h2.54V9.797c0-2.506 1.492-3.89 3.777-3.89 1.094 0 2.238.195 2.238.195v2.46h-1.26c-1.243 0-1.63.771-1.63 1.562V12h2.773l-.443 2.89h-2.33v6.988C18.343 21.128 22 16.991 22 12"
							></path>
						</svg>
					</a>
					<a href="#twitter" class="text-gray-400 hover:text-white">
						<span class="sr-only">Twitter</span>
						<svg class="h-6 w-6" fill="currentColor" viewBox="0 0 24 24">
							<path
								d="M8.29 20.251c7.547 0 11.675-6.253 11.675-11.675 0-.178 0-.355-.012-.53A8.348 8.348 0 0022 5.92a8.19 8.19 0 01-2.357.646 4.118 4.118 0 001.804-2.27 8.224 8.224 0 01-2.605.996 4.107 4.107 0 00-6.993 3.743 11.65 11.65 0 01-8.457-4.287 4.106 4.106 0 001.27 5.477A4.072 4.072 0 012.8 9.713v.052a4.105 4.105 0 003.292 4.022 4.095 4.095 0 01-1.853.07 4.108 4.108 0 003.834 2.85A8.233 8.233 0 012 18.407a11.616 11.616 0 006.29 1.84"
							></path>
						</svg>
					</a>
					<a href="#instagram" class="text-gray-400 hover:text-white">
						<span class="sr-only">Instagram</span>
						<svg class="h-6 w-6" fill="currentColor" viewBox="0 0 24 24">
							<path
								fill-rule="evenodd"
								d="M12.315 2c2.43 0 2.784.013 3.808.06 1.064.049 1.791.218 2.427.465a4.902 4.902 0 011.772 1.153 4.902 4.902 0 011.153 1.772c.247.636.416 1.363.465 2.427.048 1.067.06 1.407.06 4.123v.08c0 2.643-.012 2.987-.06 4.043-.049 1.064-.218 1.791-.465 2.427a4.902 4.902 0 01-1.153 1.772 4.902 4.902 0 01-1.772 1.153c-.636.247-1.363.416-2.427.465-1.067.048-1.407.06-4.123.06h-.08c-2.643 0-2.987-.012-4.043-.06-1.064-.049-1.791-.218-2.427-.465a4.902 4.902 0 01-1.772-1.153 4.902 4.902 0 01-1.153-1.772c-.247-.636-.416-1.363-.465-2.427-.047-1.024-.06-1.379-.06-3.808v-.63c0-2.43.013-2.784.06-3.808.049-1.064.218-1.791.465-2.427a4.902 4.902 0 011.153-1.772A4.902 4.902 0 015.45 2.525c.636-.247 1.363-.416 2.427-.465C8.901 2.013 9.256 2 11.685 2h.63zm-.081 1.802h-.468c-2.456 0-2.784.011-3.807.058-.975.045-1.504.207-1.857.344-.467.182-.8.398-1.15.748-.35.35-.566.683-.748 1.15-.137.353-.3.882-.344 1.857-.047 1.023-.058 1.351-.058 3.807v.468c0 2.456.011 2.784.058 3.807.045.975.207 1.504.344 1.857.182.466.399.8.748 1.15.35.35.683.566 1.15.748.353.137.882.3 1.857.344 1.054.048 1.37.058 4.041.058h.08c2.597 0 2.917-.01 3.96-.058.976-.045 1.505-.207 1.858-.344.466-.182.8-.398 1.15-.748.35-.35.566-.683.748-1.15.137-.353.3-.882.344-1.857.048-1.055.058-1.37.058-4.041v-.08c0-2.597-.01-2.917-.058-3.96-.045-.976-.207-1.505-.344-1.858a3.097 3.097 0 00-.748-1.15 3.098 3.098 0 00-1.15-.748c-.353-.137-.882-.3-1.857-.344-1.023-.047-1.351-.058-3.807-.058zM12 6.865a5.135 5.135 0 110 10.27 5.135 5.135 0 010-10.27zm0 1.802a3.333 3.333 0 100 6.666 3.333 3.333 0 000-6.666zm5.338-3.205a1.2 1.2 0 110 2.4 1.2 1.2 0 010-2.4z"
								clip-rule="evenodd"
							></path>
						</svg>
					</a>
				</div>
			</div>

			<!-- Quick Links -->
			<div>
				<h4 class="mb-4 font-semibold">Discovery</h4>
				<ul class="space-y-2 text-gray-400">
					<li><a href="/clubs" class="hover:text-white">Browse Clubs</a></li>
					<li><a href="/clubs/featured" class="hover:text-white">Featured Clubs</a></li>
					<li><a href="/clubs/near-me" class="hover:text-white">Clubs Near Me</a></li>
					<li><a href="/events" class="hover:text-white">Upcoming Events</a></li>
				</ul>
			</div>

			<!-- Resources -->
			<div>
				<h4 class="mb-4 font-semibold">Resources</h4>
				<ul class="space-y-2 text-gray-400">
					<li><a href="/resources/start-club" class="hover:text-white">Start a Club</a></li>
					<li><a href="/resources/management" class="hover:text-white">Club Management</a></li>
					<li><a href="/resources/best-practices" class="hover:text-white">Best Practices</a></li>
					<li><a href="/resources/guides" class="hover:text-white">Guides & Tutorials</a></li>
				</ul>
			</div>

			<!-- Support -->
			<div>
				<h4 class="mb-4 font-semibold">Support</h4>
				<ul class="space-y-2 text-gray-400">
					<li><a href="/help" class="hover:text-white">Help Center</a></li>
					<li><a href="/contact" class="hover:text-white">Contact Us</a></li>
					<li><a href="/faq" class="hover:text-white">FAQs</a></li>
					<li>
						<a href="mailto:support@dgclubs.com" class="hover:text-white">support@dgclubs.com</a>
					</li>
				</ul>
			</div>
		</div>

		<!-- Bottom Footer -->
		<div class="mt-8 border-t border-gray-700 pt-8">
			<div class="grid gap-4 md:grid-cols-2">
				<div class="text-sm text-gray-400">
					<p>&copy; 2024 DGClubs. All rights reserved.</p>
				</div>
				<div class="flex justify-start space-x-4 text-sm text-gray-400 md:justify-end">
					<a href="/privacy" class="hover:text-white">Privacy Policy</a>
					<span class="text-gray-600">|</span>
					<a href="/terms" class="hover:text-white">Terms of Service</a>
					<span class="text-gray-600">|</span>
					<a href="/accessibility" class="hover:text-white">Accessibility</a>
					<span class="text-gray-600">|</span>
					<a href="/sitemap" class="hover:text-white">Sitemap</a>
				</div>
			</div>
		</div>

		<!-- Platform Stats -->
		<div class="mt-8 border-t border-gray-700 pt-8 text-center">
			<div class="grid grid-cols-3 gap-4">
				<div>
					<div class="text-2xl font-bold text-white">500+</div>
					<div class="text-sm text-gray-400">Active Clubs</div>
				</div>
				<div>
					<div class="text-2xl font-bold text-white">50k+</div>
					<div class="text-sm text-gray-400">Club Members</div>
				</div>
				<div>
					<div class="text-2xl font-bold text-white">1000+</div>
					<div class="text-sm text-gray-400">Monthly Events</div>
				</div>
			</div>
		</div>

		<!-- Newsletter Signup -->
		<div class="mt-8 rounded-lg bg-gray-700 p-6">
			<div class="text-center">
				<h3 class="text-lg font-semibold">Stay Updated</h3>
				<p class="mt-2 text-sm text-gray-400">
					Subscribe to our newsletter for the latest disc golf club news and updates.
				</p>
				<form class="mt-4 flex items-center justify-center gap-2">
					<input
						type="email"
						placeholder="Enter your email"
						class="w-full max-w-sm rounded-lg bg-gray-600 px-4 py-2 text-white placeholder-gray-400 focus:outline-none focus:ring-2 focus:ring-blue-500"
					/>
					<button
						type="submit"
						class="rounded-lg bg-blue-600 px-4 py-2 text-white hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-500"
					>
						Subscribe
					</button>
				</form>
			</div>
		</div>
	</div>
</footer>
