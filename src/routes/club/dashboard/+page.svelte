<script>
	/** @type {{ data: import('./$types').PageData }} */
	let { data } = $props();

	import { onMount } from 'svelte';

	// State management
	let userProfile = $state({
		name: 'John Smith',
		memberSince: '2023',
		pdgaNumber: '123456',
		membershipStatus: 'Active',
		duesStatus: 'Paid',
		currentRating: 935
	});

	let upcomingEvents = $state([
		{
			id: 1,
			name: 'Weekly Singles League',
			date: 'Tomorrow at 5:30 PM',
			location: 'River Valley Course',
			registered: true
		},
		{
			id: 2,
			name: 'Monthly Tournament',
			date: 'Nov 18, 9:00 AM',
			location: 'Both Courses',
			registered: true
		}
	]);

	let recentScores = $state([
		{
			date: '2024-11-10',
			course: 'River Valley',
			score: 65,
			par: 54,
			type: 'League'
		},
		{
			date: '2024-11-03',
			course: 'Hillside',
			score: 28,
			par: 27,
			type: 'Casual'
		}
	]);

	let clubAnnouncements = $state([
		{
			id: 1,
			title: '2024 Board Elections',
			content:
				'Nominations are now open for the 2024 board positions. Submit your nominations by December 1st.',
			date: '2 days ago',
			priority: 'high'
		},
		{
			id: 2,
			title: 'Winter League Registration',
			content:
				'Winter league registration opens next week. Early bird pricing available until November 30th.',
			date: '3 days ago',
			priority: 'medium'
		}
	]);

	let personalNotifications = $state([
		{
			id: 1,
			type: 'reminder',
			message: 'Your next league round is tomorrow at 5:30 PM'
		},
		{
			id: 2,
			type: 'achievement',
			message: 'New personal best at River Valley Course!'
		}
	]);
</script>

<div class="min-h-screen bg-gray-50 p-6">
	<!-- Header with Member Info -->
	<div class="mb-8 rounded-lg bg-white p-6 shadow-sm">
		<div class="flex items-center justify-between">
			<div>
				<h1 class="text-2xl font-bold text-gray-800">Welcome, {userProfile.name}</h1>
				<p class="text-gray-600">
					Member since {userProfile.memberSince} | PDGA #{userProfile.pdgaNumber}
				</p>
			</div>
			<div class="text-right">
				<p class="font-medium text-green-600">Membership: {userProfile.membershipStatus}</p>
				<p class="text-sm text-gray-600">Current Rating: {userProfile.currentRating}</p>
			</div>
		</div>
	</div>

	<!-- Personal Notifications -->
	<div class="mb-8 space-y-4">
		{#each personalNotifications as notification}
			<div class="rounded-lg border bg-white p-4 shadow-sm">
				<div class="flex items-center">
					<div class="flex-shrink-0">
						{#if notification.type === 'reminder'}
							<div class="rounded-full bg-blue-100 p-2">
								<svg
									class="h-5 w-5 text-blue-600"
									fill="none"
									stroke="currentColor"
									viewBox="0 0 24 24"
								>
									<path
										stroke-linecap="round"
										stroke-linejoin="round"
										stroke-width="2"
										d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"
									/>
								</svg>
							</div>
						{:else if notification.type === 'achievement'}
							<div class="rounded-full bg-green-100 p-2">
								<svg
									class="h-5 w-5 text-green-600"
									fill="none"
									stroke="currentColor"
									viewBox="0 0 24 24"
								>
									<path
										stroke-linecap="round"
										stroke-linejoin="round"
										stroke-width="2"
										d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"
									/>
								</svg>
							</div>
						{/if}
					</div>
					<div class="ml-3">
						<p class="text-gray-800">{notification.message}</p>
					</div>
				</div>
			</div>
		{/each}
	</div>

	<div class="grid gap-6 lg:grid-cols-2">
		<!-- Your Upcoming Events -->
		<div class="rounded-lg bg-white p-6 shadow-sm">
			<div class="mb-4 flex items-center justify-between">
				<h2 class="text-lg font-semibold">Your Upcoming Events</h2>
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
								<p class="text-sm text-gray-500">{event.date} at {event.location}</p>
							</div>
							{#if event.registered}
								<span
									class="rounded-full bg-green-100 px-3 py-1 text-sm font-medium text-green-800"
								>
									Registered
								</span>
							{:else}
								<button
									class="rounded-lg bg-blue-600 px-4 py-2 text-sm text-white hover:bg-blue-700"
								>
									Register
								</button>
							{/if}
						</div>
					</div>
				{/each}
			</div>
		</div>

		<!-- Recent Scores -->
		<div class="rounded-lg bg-white p-6 shadow-sm">
			<div class="mb-4 flex items-center justify-between">
				<h2 class="text-lg font-semibold">Recent Scores</h2>
				<a href="/scores" class="text-sm font-medium text-blue-600 hover:text-blue-700"
					>View All Scores</a
				>
			</div>
			<div class="space-y-4">
				{#each recentScores as score}
					<div class="rounded-lg border border-gray-200 p-4">
						<div class="flex items-center justify-between">
							<div>
								<h3 class="font-medium">{score.course}</h3>
								<p class="text-sm text-gray-500">{score.date} • {score.type}</p>
							</div>
							<div class="text-right">
								<p class="text-lg font-semibold">{score.score}</p>
								<p class="text-sm text-gray-500">Par {score.par}</p>
							</div>
						</div>
					</div>
				{/each}
				<button
					class="w-full rounded-lg bg-gray-100 py-2 text-sm font-medium text-gray-600 hover:bg-gray-200"
				>
					Submit New Score
				</button>
			</div>
		</div>
	</div>

	<!-- Course Status -->
	<div class="mt-6 rounded-lg bg-white p-6 shadow-sm">
		<h2 class="mb-4 text-lg font-semibold">Course Status</h2>
		<div class="grid gap-4 md:grid-cols-2">
			<!-- River Valley Course -->
			<div class="rounded-lg border border-gray-200 p-4">
				<div class="flex items-center justify-between">
					<div>
						<h3 class="font-medium">River Valley Course</h3>
						<p class="text-sm text-gray-500">18 holes | Par 54</p>
					</div>
					<span class="rounded-full bg-green-100 px-3 py-1 text-sm font-medium text-green-800">
						Open
					</span>
				</div>
				<div class="mt-4 space-y-2 text-sm text-gray-600">
					<p>• Current layout: Long tees</p>
					<p>• Weather: 65°F, Partly Cloudy</p>
					<p>• Last updated: 1 hour ago</p>
				</div>
			</div>

			<!-- Hillside Course -->
			<div class="rounded-lg border border-gray-200 p-4">
				<div class="flex items-center justify-between">
					<div>
						<h3 class="font-medium">Hillside Course</h3>
						<p class="text-sm text-gray-500">9 holes | Par 27</p>
					</div>
					<span class="rounded-full bg-yellow-100 px-3 py-1 text-sm font-medium text-yellow-800">
						Limited
					</span>
				</div>
				<div class="mt-4 space-y-2 text-sm text-gray-600">
					<p>• Holes 7-9 closed for maintenance</p>
					<p>• Weather: 65°F, Partly Cloudy</p>
					<p>• Last updated: 1 hour ago</p>
				</div>
			</div>
		</div>
	</div>

	<!-- Club Announcements -->
	<div class="mt-6 rounded-lg bg-white p-6 shadow-sm">
		<h2 class="mb-4 text-lg font-semibold">Club Announcements</h2>
		<div class="space-y-4">
			{#each clubAnnouncements as announcement}
				<div class="rounded-lg border border-gray-200 p-4">
					<div class="flex items-center justify-between">
						<h3 class="font-medium">{announcement.title}</h3>
						<span class="text-sm text-gray-500">{announcement.date}</span>
					</div>
					<p class="mt-2 text-gray-600">{announcement.content}</p>
				</div>
			{/each}
		</div>
	</div>

	<!-- Quick Actions -->
	<div class="mt-6 grid gap-4 sm:grid-cols-2 lg:grid-cols-4">
		<a href="/profile" class="rounded-lg bg-blue-50 p-4 text-center hover:bg-blue-100">
			<svg
				class="mx-auto h-6 w-6 text-blue-600"
				fill="none"
				stroke="currentColor"
				viewBox="0 0 24 24"
			>
				<path
					stroke-linecap="round"
					stroke-linejoin="round"
					stroke-width="2"
					d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z"
				/>
			</svg>
			<span class="mt-2 block font-medium">Update Profile</span>
		</a>

		<a href="/members" class="rounded-lg bg-green-50 p-4 text-center hover:bg-green-100">
			<svg
				class="mx-auto h-6 w-6 text-green-600"
				fill="none"
				stroke="currentColor"
				viewBox="0 0 24 24"
			>
				<path
					stroke-linecap="round"
					stroke-linejoin="round"
					stroke-width="2"
					d="M17 20h5v-2a3 3 0 00-5.356-1.857M17 20H7m10 0v-2c0-.656-.126-1.283-.356-1.857M7 20H2v-2a3 3 0 015.356-1.857M7 20v-2c0-.656.126-1.283.356-1.857m0 0a5.002 5.002 0 019.288 0M15 7a3 3 0 11-6 0 3 3 0 016 0zm6 3a2 2 0 11-4 0 2 2 0 014 0zM7 10a2 2 0 11-4 0 2 2 0 014 0z"
				/>
			</svg>
			<span class="mt-2 block font-medium">Member Directory</span>
		</a>

		<a href="/leagues" class="rounded-lg bg-purple-50 p-4 text-center hover:bg-purple-100">
			<svg
				class="mx-auto h-6 w-6 text-purple-600"
				fill="none"
				stroke="currentColor"
				viewBox="0 0 24 24"
			>
				<path
					stroke-linecap="round"
					stroke-linejoin="round"
					stroke-width="2"
					d="M9 19v-6a2 2 0 00-2-2H5a2 2 0 00-2 2v6a2 2 0 002 2h2a2 2 0 002-2zm0 0V9a2 2 0 012-2h2a2 2 0 012 2v10m-6 0a2 2 0 002 2h2a2 2 0 002-2m0 0V5a2 2 0 012-2h2a2 2 0 012 2v14a2 2 0 01-2 2h-2a2 2 0 01-2-2z"
				/>
			</svg>
			<span class="mt-2 block font-medium">League Standings</span>
		</a>

		<a href="/support" class="rounded-lg bg-red-50 p-4 text-center hover:bg-red-100">
			<svg
				class="mx-auto h-6 w-6 text-red-600"
				fill="none"
				stroke="currentColor"
				viewBox="0 0 24 24"
			>
				<path
					stroke-linecap="round"
					stroke-linejoin="round"
					stroke-width="2"
					d="M8 12h.01M12 12h.01M16 12h.01M21 12c0 4.418-4.03 8-9 8a9.863 9.863 0 01-4.255-.949L3 20l1.395-3.72C3.512 15.042 3 13.574 3 12c0-4.418 4.03-8 9-8s9 3.582 9 8z"
				/>
			</svg>
			<span class="mt-2 block font-medium">Contact Support</span>
		</a>
	</div>
</div>
