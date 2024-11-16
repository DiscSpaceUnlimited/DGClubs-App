<script>
	/** @type {{ data: import('./$types').PageData }} */
	let { data } = $props();

	import { onMount } from 'svelte';

	// Club statistics
	let clubStats = $state({
		totalMembers: 180,
		activeCourses: 2,
		upcomingEvents: 5,
		yearsActive: 15,
		lastUpdated: '2024-11-14'
	});

	// Upcoming events
	let upcomingEvents = $state([
		{
			name: 'Weekly Singles League',
			date: 'Every Thursday at 5:30 PM',
			location: 'River Valley Course',
			type: 'League',
			registrationStatus: 'Open'
		},
		{
			name: 'Monthly Tournament',
			date: 'Nov 18, 2024 9:00 AM',
			location: 'Both Courses',
			type: 'Tournament',
			registrationStatus: 'Filling Fast'
		},
		{
			name: 'Beginner Clinic',
			date: 'Nov 20, 2024 10:00 AM',
			location: 'River Valley Course',
			type: 'Clinic',
			registrationStatus: 'Open'
		}
	]);

	// News and announcements
	let clubNews = $state([
		{
			title: 'Winter League Registration Opening Soon',
			date: '2024-11-13',
			preview:
				'Get ready for our annual Winter League series! Registration opens next week with early bird pricing available until November 30th.',
			category: 'Events'
		},
		{
			title: 'Course Improvement Project Completed',
			date: '2024-11-12',
			preview:
				"Thanks to our volunteers, we've completed the installation of new tee pads on holes 7-9 at the Hillside course.",
			category: 'Course Updates'
		},
		{
			title: 'Club Achievement: PDGA Affiliate of the Month',
			date: '2024-11-10',
			preview:
				"We're proud to announce that our club has been recognized as the PDGA Affiliate Club of the Month!",
			category: 'News'
		}
	]);

	// Course information
	let courseStatus = $state([
		{
			name: 'River Valley Course',
			status: 'Open',
			holes: 18,
			par: 54,
			difficulty: 'Intermediate',
			lastUpdate: '1 hour ago',
			currentConditions: {
				weather: '65°F, Partly Cloudy',
				teePads: 'Clear',
				fairways: 'Good',
				baskets: 'Excellent'
			}
		},
		{
			name: 'Hillside Course',
			status: 'Limited Access',
			holes: 9,
			par: 27,
			difficulty: 'Beginner Friendly',
			lastUpdate: '2 hours ago',
			currentConditions: {
				weather: '65°F, Partly Cloudy',
				teePads: 'Good',
				fairways: 'Under Maintenance',
				baskets: 'Good'
			}
		}
	]);

	// Recent tournament results
	let recentResults = $state([
		{
			name: 'October Monthly',
			date: '2024-10-15',
			winners: {
				pro: 'Mike Johnson (-8)',
				advanced: 'Sarah Smith (-2)',
				recreational: 'Tom Wilson (+3)'
			},
			participants: 64
		},
		{
			name: 'Fall Classic',
			date: '2024-10-01',
			winners: {
				pro: 'Chris Davis (-10)',
				advanced: 'Lisa Brown (-4)',
				recreational: 'James Wilson (+2)'
			},
			participants: 86
		}
	]);
</script>

<div class="min-h-screen bg-gray-50 p-6">
	<!-- Club Header -->
	<div class="mb-8 rounded-lg bg-white p-6 shadow-sm">
		<div class="flex flex-col items-center justify-between gap-4 md:flex-row">
			<div>
				<h1 class="text-2xl font-bold text-gray-800">River Valley Disc Golf Club</h1>
				<p class="text-gray-600">Your premier disc golf destination since 2009</p>
			</div>
			<div class="flex gap-4">
				<a
					href="/join"
					class="rounded-lg bg-blue-600 px-6 py-2 text-white transition-colors hover:bg-blue-700"
				>
					Join the Club
				</a>
				<a
					href="/contact"
					class="rounded-lg border border-blue-600 px-6 py-2 text-blue-600 transition-colors hover:bg-blue-50"
				>
					Contact Us
				</a>
			</div>
		</div>
	</div>

	<!-- Club Stats -->
	<div class="mb-8 grid gap-4 sm:grid-cols-2 lg:grid-cols-4">
		<div class="rounded-lg bg-white p-6 shadow-sm">
			<div class="text-sm font-medium text-gray-500">Total Members</div>
			<div class="mt-2 text-3xl font-bold">{clubStats.totalMembers}</div>
		</div>
		<div class="rounded-lg bg-white p-6 shadow-sm">
			<div class="text-sm font-medium text-gray-500">Active Courses</div>
			<div class="mt-2 text-3xl font-bold">{clubStats.activeCourses}</div>
		</div>
		<div class="rounded-lg bg-white p-6 shadow-sm">
			<div class="text-sm font-medium text-gray-500">Upcoming Events</div>
			<div class="mt-2 text-3xl font-bold">{clubStats.upcomingEvents}</div>
		</div>
		<div class="rounded-lg bg-white p-6 shadow-sm">
			<div class="text-sm font-medium text-gray-500">Years Active</div>
			<div class="mt-2 text-3xl font-bold">{clubStats.yearsActive}</div>
		</div>
	</div>

	<!-- Main Content Grid -->
	<div class="grid gap-6 lg:grid-cols-2">
		<!-- Upcoming Events -->
		<div class="rounded-lg bg-white p-6 shadow-sm">
			<div class="mb-4 flex items-center justify-between">
				<h2 class="text-lg font-semibold">Upcoming Events</h2>
				<a href="/events" class="text-sm font-medium text-blue-600 hover:text-blue-700"
					>View All Events</a
				>
			</div>
			<div class="space-y-4">
				{#each upcomingEvents as event}
					<div class="rounded-lg border border-gray-200 p-4">
						<div class="flex items-center justify-between">
							<div>
								<h3 class="font-medium">{event.name}</h3>
								<p class="text-sm text-gray-500">
									{event.date} at {event.location}
								</p>
							</div>
							<div class="text-right">
								<span
									class="rounded-full px-3 py-1 text-sm font-medium {event.registrationStatus ===
									'Open'
										? 'bg-green-100 text-green-800'
										: 'bg-yellow-100 text-yellow-800'}"
								>
									{event.registrationStatus}
								</span>
							</div>
						</div>
					</div>
				{/each}
				<a
					href="/events/register"
					class="block w-full rounded-lg bg-gray-100 py-2 text-center text-sm font-medium text-gray-600 hover:bg-gray-200"
				>
					Register for an Event
				</a>
			</div>
		</div>

		<!-- News and Announcements -->
		<div class="rounded-lg bg-white p-6 shadow-sm">
			<div class="mb-4 flex items-center justify-between">
				<h2 class="text-lg font-semibold">Club News</h2>
				<a href="/news" class="text-sm font-medium text-blue-600 hover:text-blue-700"
					>View All News</a
				>
			</div>
			<div class="space-y-4">
				{#each clubNews as news}
					<div class="rounded-lg border border-gray-200 p-4">
						<div class="flex items-center justify-between">
							<span class="rounded-full bg-blue-100 px-2 py-1 text-xs font-medium text-blue-800">
								{news.category}
							</span>
							<span class="text-sm text-gray-500">{news.date}</span>
						</div>
						<h3 class="mt-2 font-medium">{news.title}</h3>
						<p class="mt-1 text-sm text-gray-600">{news.preview}</p>
					</div>
				{/each}
			</div>
		</div>

		<!-- Course Status -->
		<div class="rounded-lg bg-white p-6 shadow-sm">
			<h2 class="mb-4 text-lg font-semibold">Course Status</h2>
			<div class="space-y-6">
				{#each courseStatus as course}
					<div class="rounded-lg border border-gray-200 p-4">
						<div class="flex items-center justify-between">
							<div>
								<h3 class="font-medium">{course.name}</h3>
								<p class="text-sm text-gray-500">
									{course.holes} holes | Par {course.par} | {course.difficulty}
								</p>
							</div>
							<span
								class="rounded-full px-3 py-1 text-sm font-medium {course.status === 'Open'
									? 'bg-green-100 text-green-800'
									: 'bg-yellow-100 text-yellow-800'}"
							>
								{course.status}
							</span>
						</div>
						<div class="mt-4 grid grid-cols-2 gap-4 text-sm">
							<div>
								<p class="font-medium">Current Conditions:</p>
								<ul class="mt-1 space-y-1 text-gray-600">
									<li>Weather: {course.currentConditions.weather}</li>
									<li>Tee Pads: {course.currentConditions.teePads}</li>
									<li>Fairways: {course.currentConditions.fairways}</li>
									<li>Baskets: {course.currentConditions.baskets}</li>
								</ul>
							</div>
							<div class="text-right">
								<p class="text-gray-500">Last updated: {course.lastUpdate}</p>
								<a
									href="/courses/{course.name.toLowerCase().replace(' ', '-')}"
									class="mt-2 inline-block text-blue-600 hover:text-blue-700"
								>
									View Course Details →
								</a>
							</div>
						</div>
					</div>
				{/each}
			</div>
		</div>

		<!-- Recent Tournament Results -->
		<div class="rounded-lg bg-white p-6 shadow-sm">
			<div class="mb-4 flex items-center justify-between">
				<h2 class="text-lg font-semibold">Recent Tournament Results</h2>
				<a href="/tournaments" class="text-sm font-medium text-blue-600 hover:text-blue-700"
					>View All Results</a
				>
			</div>
			<div class="space-y-4">
				{#each recentResults as result}
					<div class="rounded-lg border border-gray-200 p-4">
						<div class="flex items-center justify-between">
							<h3 class="font-medium">{result.name}</h3>
							<span class="text-sm text-gray-500">{result.date}</span>
						</div>
						<div class="mt-2 grid grid-cols-3 gap-4 text-sm">
							<div>
								<p class="font-medium">Pro</p>
								<p class="text-gray-600">{result.winners.pro}</p>
							</div>
							<div>
								<p class="font-medium">Advanced</p>
								<p class="text-gray-600">{result.winners.advanced}</p>
							</div>
							<div>
								<p class="font-medium">Recreational</p>
								<p class="text-gray-600">{result.winners.recreational}</p>
							</div>
						</div>
						<p class="mt-2 text-sm text-gray-500">{result.participants} participants</p>
					</div>
				{/each}
			</div>
		</div>
	</div>

	<!-- Call to Action -->
	<div class="mt-8 rounded-lg bg-blue-50 p-8 text-center">
		<h2 class="mb-4 text-2xl font-bold text-gray-800">Join Our Community</h2>
		<p class="mb-6 text-gray-600">
			Become a member today and enjoy exclusive benefits, tournament access, and more!
		</p>
		<div class="flex justify-center gap-4">
			<a
				href="/membership"
				class="rounded-lg bg-blue-600 px-6 py-2 text-white transition-colors hover:bg-blue-700"
			>
				View Membership Options
			</a>
			<a
				href="/learn-more"
				class="rounded-lg border border-blue-600 px-6 py-2 text-blue-600 transition-colors hover:bg-blue-50"
			>
				Learn More
			</a>
		</div>
	</div>
</div>
