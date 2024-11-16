<script>
	/** @type {{ data: import('./$types').PageData }} */
	let { data } = $props();

	import { onMount } from 'svelte';

	// Admin metrics
	let adminMetrics = $state({
		totalMembers: 180,
		membersTrend: '+12% this month',
		activeEvents: 5,
		pendingApprovals: 3,
		monthlyRevenue: '$2,450',
		revenueTrend: '+8%',
		maintenanceTickets: 4,
		volunteerSignups: 12
	});

	// Pending approvals/actions
	let pendingActions = $state([
		{
			type: 'member',
			action: 'New Member Application',
			name: 'Sarah Johnson',
			date: '2024-11-14',
			details: 'PDGA #123456'
		},
		{
			type: 'payment',
			action: 'Refund Request',
			name: 'Tournament Registration',
			date: '2024-11-13',
			details: '$45.00'
		},
		{
			type: 'event',
			action: 'Event Approval',
			name: 'Winter League Series',
			date: '2024-11-13',
			details: 'Dec 2024 - Mar 2025'
		}
	]);

	// System alerts
	let systemAlerts = $state([
		{
			priority: 'high',
			message: 'Monthly Tournament reaching capacity (85%)',
			timestamp: '1 hour ago'
		},
		{
			priority: 'medium',
			message: 'Course maintenance needed on hole 7',
			timestamp: '3 hours ago'
		},
		{
			priority: 'low',
			message: '15 membership renewals due next week',
			timestamp: '5 hours ago'
		}
	]);

	// Financial overview
	let financialMetrics = $state({
		currentMonth: {
			revenue: 2450,
			expenses: 1200,
			memberships: 850,
			events: 1200,
			merchandise: 400
		},
		pendingPayments: 3,
		overduePayments: 1
	});

	// Event management data
	let eventMetrics = $state({
		upcoming: 5,
		pendingApproval: 2,
		needsVolunteers: 3,
		registrationIssues: 1
	});

	// Course management data
	let courseStatus = $state([
		{
			name: 'River Valley Course',
			status: 'open',
			maintenanceNeeded: 2,
			lastInspection: '2024-11-12',
			nextMaintenance: '2024-11-20'
		},
		{
			name: 'Hillside Course',
			status: 'limited',
			maintenanceNeeded: 4,
			lastInspection: '2024-11-10',
			nextMaintenance: '2024-11-15'
		}
	]);
</script>

<div class="min-h-screen bg-gray-50 p-6">
	<!-- Admin Header -->
	<div class="mb-8 flex items-center justify-between">
		<div>
			<h1 class="text-2xl font-bold text-gray-800">Club Administration</h1>
			<p class="text-gray-600">Managing River Valley Disc Golf Club</p>
		</div>
		<div class="flex gap-4">
			<button class="rounded-lg bg-blue-600 px-4 py-2 text-white hover:bg-blue-700">
				System Settings
			</button>
			<button class="rounded-lg bg-white px-4 py-2 text-gray-600 hover:bg-gray-50">
				Generate Reports
			</button>
		</div>
	</div>

	<!-- System Alerts -->
	<div class="mb-8 space-y-4">
		{#each systemAlerts as alert}
			<div
				class="rounded-lg border p-4 {alert.priority === 'high'
					? 'border-red-200 bg-red-50'
					: alert.priority === 'medium'
						? 'border-yellow-200 bg-yellow-50'
						: 'border-blue-200 bg-blue-50'}"
			>
				<div class="flex items-center justify-between">
					<div class="flex items-center">
						<svg
							class="h-5 w-5 {alert.priority === 'high'
								? 'text-red-500'
								: alert.priority === 'medium'
									? 'text-yellow-500'
									: 'text-blue-500'}"
							fill="none"
							stroke="currentColor"
							viewBox="0 0 24 24"
						>
							<path
								stroke-linecap="round"
								stroke-linejoin="round"
								stroke-width="2"
								d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z"
							/>
						</svg>
						<span class="ml-3">{alert.message}</span>
					</div>
					<span class="text-sm text-gray-500">{alert.timestamp}</span>
				</div>
			</div>
		{/each}
	</div>

	<!-- Quick Stats Grid -->
	<div class="mb-8 grid gap-6 sm:grid-cols-2 lg:grid-cols-4">
		<!-- Member Stats -->
		<div class="rounded-lg bg-white p-6 shadow-sm">
			<div class="flex items-center justify-between">
				<h3 class="text-sm font-medium text-gray-500">Total Members</h3>
				<span class="text-sm text-green-600">{adminMetrics.membersTrend}</span>
			</div>
			<p class="mt-2 text-3xl font-bold">{adminMetrics.totalMembers}</p>
			<div class="mt-4 flex justify-between text-sm">
				<span>Active: 156</span>
				<span>Pending: {adminMetrics.pendingApprovals}</span>
			</div>
		</div>

		<!-- Event Stats -->
		<div class="rounded-lg bg-white p-6 shadow-sm">
			<div class="flex items-center justify-between">
				<h3 class="text-sm font-medium text-gray-500">Event Management</h3>
			</div>
			<p class="mt-2 text-3xl font-bold">{eventMetrics.upcoming}</p>
			<div class="mt-4 flex justify-between text-sm">
				<span>Pending: {eventMetrics.pendingApproval}</span>
				<span>Needs Volunteers: {eventMetrics.needsVolunteers}</span>
			</div>
		</div>

		<!-- Financial Stats -->
		<div class="rounded-lg bg-white p-6 shadow-sm">
			<div class="flex items-center justify-between">
				<h3 class="text-sm font-medium text-gray-500">Monthly Revenue</h3>
				<span class="text-sm text-green-600">{adminMetrics.revenueTrend}</span>
			</div>
			<p class="mt-2 text-3xl font-bold">{adminMetrics.monthlyRevenue}</p>
			<div class="mt-4 flex justify-between text-sm">
				<span>Pending: {financialMetrics.pendingPayments}</span>
				<span>Overdue: {financialMetrics.overduePayments}</span>
			</div>
		</div>

		<!-- Maintenance Stats -->
		<div class="rounded-lg bg-white p-6 shadow-sm">
			<div class="flex items-center justify-between">
				<h3 class="text-sm font-medium text-gray-500">Maintenance Items</h3>
			</div>
			<p class="mt-2 text-3xl font-bold">{adminMetrics.maintenanceTickets}</p>
			<div class="mt-4 flex justify-between text-sm">
				<span>Volunteers: {adminMetrics.volunteerSignups}</span>
				<span>High Priority: 2</span>
			</div>
		</div>
	</div>

	<!-- Main Content Grid -->
	<div class="grid gap-6 lg:grid-cols-2">
		<!-- Pending Actions -->
		<div class="rounded-lg bg-white p-6 shadow-sm">
			<h2 class="mb-4 text-lg font-semibold">Pending Actions</h2>
			<div class="space-y-4">
				{#each pendingActions as action}
					<div class="rounded-lg border border-gray-200 p-4">
						<div class="flex items-center justify-between">
							<div>
								<span
									class="inline-block rounded-full px-2 py-1 text-xs font-semibold {action.type ===
									'member'
										? 'bg-blue-100 text-blue-800'
										: action.type === 'payment'
											? 'bg-green-100 text-green-800'
											: 'bg-purple-100 text-purple-800'}"
								>
									{action.type.toUpperCase()}
								</span>
								<h3 class="mt-2 font-medium">{action.action}</h3>
								<p class="text-sm text-gray-500">{action.name} - {action.details}</p>
							</div>
							<div class="flex space-x-2">
								<button
									class="rounded bg-blue-50 px-3 py-1 text-sm font-medium text-blue-600 hover:bg-blue-100"
								>
									Review
								</button>
								<button
									class="rounded bg-gray-50 px-3 py-1 text-sm font-medium text-gray-600 hover:bg-gray-100"
								>
									Dismiss
								</button>
							</div>
						</div>
					</div>
				{/each}
			</div>
		</div>

		<!-- Course Management -->
		<div class="rounded-lg bg-white p-6 shadow-sm">
			<h2 class="mb-4 text-lg font-semibold">Course Management</h2>
			<div class="space-y-4">
				{#each courseStatus as course}
					<div class="rounded-lg border border-gray-200 p-4">
						<div class="flex items-center justify-between">
							<div>
								<div class="flex items-center gap-2">
									<h3 class="font-medium">{course.name}</h3>
									<span
										class="rounded-full px-2 py-1 text-xs font-semibold {course.status === 'open'
											? 'bg-green-100 text-green-800'
											: 'bg-yellow-100 text-yellow-800'}"
									>
										{course.status.toUpperCase()}
									</span>
								</div>
								<p class="mt-1 text-sm text-gray-500">
									{course.maintenanceNeeded} maintenance items | Next scheduled: {course.nextMaintenance}
								</p>
							</div>
							<button
								class="rounded bg-blue-50 px-3 py-1 text-sm font-medium text-blue-600 hover:bg-blue-100"
							>
								Manage
							</button>
						</div>
					</div>
				{/each}
			</div>
		</div>

		<!-- Financial Overview -->
		<div class="rounded-lg bg-white p-6 shadow-sm">
			<h2 class="mb-4 text-lg font-semibold">Financial Overview</h2>
			<div class="space-y-4">
				<div class="grid grid-cols-2 gap-4">
					<div class="rounded-lg bg-gray-50 p-4">
						<h4 class="text-sm font-medium text-gray-500">Revenue Breakdown</h4>
						<div class="mt-2 space-y-2">
							<div class="flex justify-between">
								<span>Memberships</span>
								<span>${financialMetrics.currentMonth.memberships}</span>
							</div>
							<div class="flex justify-between">
								<span>Events</span>
								<span>${financialMetrics.currentMonth.events}</span>
							</div>
							<div class="flex justify-between">
								<span>Merchandise</span>
								<span>${financialMetrics.currentMonth.merchandise}</span>
							</div>
						</div>
					</div>
					<div class="rounded-lg bg-gray-50 p-4">
						<h4 class="text-sm font-medium text-gray-500">Monthly Summary</h4>
						<div class="mt-2 space-y-2">
							<div class="flex justify-between">
								<span>Total Revenue</span>
								<span>${financialMetrics.currentMonth.revenue}</span>
							</div>
							<div class="flex justify-between">
								<span>Total Expenses</span>
								<span>${financialMetrics.currentMonth.expenses}</span>
							</div>
							<div class="flex justify-between font-medium">
								<span>Net</span>
								<span
									>${financialMetrics.currentMonth.revenue -
										financialMetrics.currentMonth.expenses}</span
								>
							</div>
						</div>
					</div>
				</div>
				<button
					class="w-full rounded-lg bg-blue-50 py-2 text-sm font-medium text-blue-600 hover:bg-blue-100"
				>
					View Detailed Reports
				</button>
			</div>
		</div>

		<!-- Event Management -->
		<div class="rounded-lg bg-white p-6 shadow-sm">
			<h2 class="mb-4 text-lg font-semibold">Event Management</h2>
			<div class="space-y-4">
				<div class="grid grid-cols-2 gap-4">
					<div class="rounded-lg bg-gray-50 p-4">
						<h4 class="text-sm font-medium text-gray-500">Upcoming Events</h4>
						<p class="mt-2 text-2xl font-semibold">{eventMetrics.upcoming}</p>
						<div class="mt-2 space-y-1 text-sm">
							<p>• {eventMetrics.pendingApproval} pending approval</p>
							<p>• {eventMetrics.needsVolunteers} need volunteers</p>
							<p>• {eventMetrics.registrationIssues} registration issues</p>
						</div>
					</div>
					<div class="rounded-lg bg-gray-50 p-4">
						<h4 class="text-sm font-medium text-gray-500">Quick Actions</h4>
						<div class="mt-2 space-y-2">
							<button
								class="w-full rounded bg-blue-100 px-3 py-1 text-sm font-medium text-blue-600 hover:bg-blue-200"
							>
								Create Event
							</button>
							<button
								class="w-full rounded bg-green-100 px-3 py-1 text-sm font-medium text-green-600 hover:bg-green-200"
							>
								Manage Calendar
							</button>
							<button
								class="w-full rounded bg-purple-100 px-3 py-1 text-sm font-medium text-purple-600 hover:bg-purple-200"
							>
								Schedule Volunteers
							</button>
							<button
								class="w-full rounded bg-yellow-100 px-3 py-1 text-sm font-medium text-yellow-600 hover:bg-yellow-200"
							>
								Review Registrations
							</button>
						</div>
					</div>
				</div>
			</div>
		</div>
	</div>

	<!-- Additional Administrative Sections -->
	<div class="mt-6 grid gap-6 lg:grid-cols-2">
		<!-- Member Management -->
		<div class="rounded-lg bg-white p-6 shadow-sm">
			<h2 class="mb-4 text-lg font-semibold">Member Management</h2>
			<div class="space-y-4">
				<div class="flex items-center justify-between">
					<div class="flex gap-2">
						<button
							class="rounded-lg bg-blue-50 px-4 py-2 text-sm font-medium text-blue-600 hover:bg-blue-100"
						>
							Add Member
						</button>
						<button
							class="rounded-lg bg-gray-50 px-4 py-2 text-sm font-medium text-gray-600 hover:bg-gray-100"
						>
							Export List
						</button>
					</div>
					<div class="relative">
						<input
							type="search"
							placeholder="Search members..."
							class="rounded-lg border border-gray-200 px-4 py-2 text-sm"
						/>
					</div>
				</div>

				<div class="rounded-lg border border-gray-200">
					<div class="overflow-x-auto">
						<table class="min-w-full divide-y divide-gray-200">
							<thead class="bg-gray-50">
								<tr>
									<th
										class="px-6 py-3 text-left text-xs font-medium uppercase tracking-wider text-gray-500"
									>
										Member
									</th>
									<th
										class="px-6 py-3 text-left text-xs font-medium uppercase tracking-wider text-gray-500"
									>
										Status
									</th>
									<th
										class="px-6 py-3 text-left text-xs font-medium uppercase tracking-wider text-gray-500"
									>
										Membership
									</th>
									<th
										class="px-6 py-3 text-left text-xs font-medium uppercase tracking-wider text-gray-500"
									>
										Actions
									</th>
								</tr>
							</thead>
							<tbody class="divide-y divide-gray-200 bg-white">
								<tr>
									<td class="whitespace-nowrap px-6 py-4">
										<div class="flex items-center">
											<div>
												<div class="font-medium">John Smith</div>
												<div class="text-sm text-gray-500">PDGA #123456</div>
											</div>
										</div>
									</td>
									<td class="whitespace-nowrap px-6 py-4">
										<span
											class="rounded-full bg-green-100 px-2 py-1 text-xs font-medium text-green-800"
										>
											Active
										</span>
									</td>
									<td class="whitespace-nowrap px-6 py-4 text-sm text-gray-500">
										Annual (Expires: Dec 2024)
									</td>
									<td class="whitespace-nowrap px-6 py-4 text-sm">
										<button class="text-blue-600 hover:text-blue-900">Edit</button>
									</td>
								</tr>
								<!-- Add more member rows as needed -->
							</tbody>
						</table>
					</div>
				</div>
			</div>
		</div>

		<!-- Communication Center -->
		<div class="rounded-lg bg-white p-6 shadow-sm">
			<h2 class="mb-4 text-lg font-semibold">Communication Center</h2>
			<div class="space-y-4">
				<div class="grid grid-cols-2 gap-4">
					<!-- Email Templates -->
					<div class="rounded-lg bg-gray-50 p-4">
						<h3 class="mb-3 text-sm font-medium text-gray-700">Email Templates</h3>
						<select class="w-full rounded-lg border border-gray-200 p-2 text-sm">
							<option>Welcome New Member</option>
							<option>Event Reminder</option>
							<option>Membership Renewal</option>
							<option>Tournament Results</option>
						</select>
						<button
							class="mt-2 w-full rounded bg-blue-600 px-4 py-2 text-sm text-white hover:bg-blue-700"
						>
							Create New Template
						</button>
					</div>

					<!-- Quick Message -->
					<div class="rounded-lg bg-gray-50 p-4">
						<h3 class="mb-3 text-sm font-medium text-gray-700">Quick Message</h3>
						<div class="space-y-2">
							<select class="w-full rounded-lg border border-gray-200 p-2 text-sm">
								<option>All Members</option>
								<option>Event Participants</option>
								<option>Board Members</option>
								<option>Volunteers</option>
							</select>
							<button
								class="w-full rounded bg-green-600 px-4 py-2 text-sm text-white hover:bg-green-700"
							>
								Send Message
							</button>
						</div>
					</div>
				</div>

				<!-- Recent Communications -->
				<div class="rounded-lg border border-gray-200 p-4">
					<h3 class="mb-3 text-sm font-medium text-gray-700">Recent Communications</h3>
					<div class="space-y-3">
						<div class="flex justify-between border-b border-gray-100 pb-2">
							<div>
								<div class="text-sm font-medium">Tournament Update</div>
								<div class="text-xs text-gray-500">
									Sent to: Event Participants (124 recipients)
								</div>
							</div>
							<div class="text-xs text-gray-500">2 hours ago</div>
						</div>
						<div class="flex justify-between border-b border-gray-100 pb-2">
							<div>
								<div class="text-sm font-medium">Course Maintenance Notice</div>
								<div class="text-xs text-gray-500">Sent to: All Members (180 recipients)</div>
							</div>
							<div class="text-xs text-gray-500">Yesterday</div>
						</div>
					</div>
				</div>
			</div>
		</div>
	</div>

	<!-- System Settings and Tools -->
	<div class="mt-6">
		<div class="rounded-lg bg-white p-6 shadow-sm">
			<h2 class="mb-4 text-lg font-semibold">System Settings & Tools</h2>
			<div class="grid gap-6 md:grid-cols-3">
				<!-- Website Management -->
				<div class="rounded-lg border border-gray-200 p-4">
					<h3 class="mb-3 text-sm font-medium text-gray-700">Website Management</h3>
					<div class="space-y-2">
						<button
							class="w-full rounded bg-gray-100 px-4 py-2 text-sm text-gray-600 hover:bg-gray-200"
						>
							Edit Home Page
						</button>
						<button
							class="w-full rounded bg-gray-100 px-4 py-2 text-sm text-gray-600 hover:bg-gray-200"
						>
							Manage News
						</button>
						<button
							class="w-full rounded bg-gray-100 px-4 py-2 text-sm text-gray-600 hover:bg-gray-200"
						>
							Update Calendar
						</button>
					</div>
				</div>

				<!-- System Configuration -->
				<div class="rounded-lg border border-gray-200 p-4">
					<h3 class="mb-3 text-sm font-medium text-gray-700">System Configuration</h3>
					<div class="space-y-2">
						<button
							class="w-full rounded bg-gray-100 px-4 py-2 text-sm text-gray-600 hover:bg-gray-200"
						>
							User Permissions
						</button>
						<button
							class="w-full rounded bg-gray-100 px-4 py-2 text-sm text-gray-600 hover:bg-gray-200"
						>
							Payment Settings
						</button>
						<button
							class="w-full rounded bg-gray-100 px-4 py-2 text-sm text-gray-600 hover:bg-gray-200"
						>
							Email Settings
						</button>
					</div>
				</div>

				<!-- Tools & Reports -->
				<div class="rounded-lg border border-gray-200 p-4">
					<h3 class="mb-3 text-sm font-medium text-gray-700">Tools & Reports</h3>
					<div class="space-y-2">
						<button
							class="w-full rounded bg-gray-100 px-4 py-2 text-sm text-gray-600 hover:bg-gray-200"
						>
							Analytics Dashboard
						</button>
						<button
							class="w-full rounded bg-gray-100 px-4 py-2 text-sm text-gray-600 hover:bg-gray-200"
						>
							Export Data
						</button>
						<button
							class="w-full rounded bg-gray-100 px-4 py-2 text-sm text-gray-600 hover:bg-gray-200"
						>
							System Logs
						</button>
					</div>
				</div>
			</div>
		</div>
	</div>
</div>