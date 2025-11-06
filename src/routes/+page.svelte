<script lang="ts">
	import AspectRatio from '$lib/components/aspect-ratio.svelte';
	import { eachDayOfInterval, addDays, format } from 'date-fns';

	const chores = {
		kitchen: [
			'Wash Dishes',
			'Load Dishwasher',
			'Empty Dishwasher',
			'Clear Countertops',
			'Wipe Countertops',
			'Clean Stove',
			'Clean Microwave',
			'Sweep Kitchen Floor',
			'Clear Dinner Table',
			'Wipe Dinner Table'
		],
		livingroom: [
			'Put Away Electronics',
			'Vacuum Couch',
			'Sweep Floor',
			'Mop Floor',
			'Dust Media Center',
			'Dust TV',
			'Wipe TV'
		]
	};
	const householdMembers = ['Rico', 'Melissa', 'Natalie', 'Ben'];

	const startDay = new Date();
	const daysToShow = 14;
	const days = eachDayOfInterval({ start: startDay, end: addDays(startDay, daysToShow) });
	const choreRows =
		Object.keys(chores).length +
		Object.values(chores).reduce((acc, chore) => acc + chore.length, 0);
	const initialBoxes = Array.from({ length: choreRows * days.length }, (_, i) => i);
</script>

<main class="flex min-h-screen w-full items-center justify-center p-4">
	<div class="flex w-full max-w-[120vh] items-center justify-center self-stretch">
		<AspectRatio ratio={11 / 8.5} class="border">
			<div class="flex h-full w-full">
				<div class="grid grow grid-cols-11 grid-rows-8">
					<div
						class="col-span-2 row-span-1 flex flex-col items-center justify-center border-r border-dashed"
					>
						<h1>Chore Logger</h1>
						<p class="text-xs opacity-75">Initial the chores you do</p>
					</div>
					<div
						style={`grid-template-columns: repeat(${days.length}, 1fr);`}
						class="col-span-9 row-span-1 grid w-full max-w-full"
					>
						<!-- Dates -->
						{#each days as day}
							<div
								class="relative flex w-full items-center justify-center border-r border-neutral-500 last:border-r-0"
							>
								<div
									class="absolute inset-0 flex -rotate-90 items-center justify-center text-xs text-nowrap"
								>
									<p>
										{format(day, 'ccc, MMM dd')}
									</p>
								</div>
							</div>
						{/each}
					</div>
					<div
						style={`grid-template-rows: repeat(${choreRows}, 1fr);`}
						class="col-span-2 row-span-7 grid border-t border-r border-dashed"
					>
						<!-- Chores -->
						{#each Object.entries(chores) as [room, roomchores]}
							<div class="flex items-center justify-center bg-neutral-500 text-white">
								<h2 class="text-sm font-semibold capitalize">{room}</h2>
							</div>
							{#each roomchores as chore}
								<div class="flex items-center border-b border-neutral-500 px-2 last:border-b-0">
									<p class="w-full text-right text-xs">{chore}</p>
								</div>
							{/each}
						{/each}
					</div>
					<div
						style={`grid-template-rows: repeat(${choreRows}, 1fr); grid-template-columns: repeat(${days.length}, 1fr);`}
						class="col-span-9 row-span-7 grid w-full border-t border-dashed"
					>
						<!-- Initial boxes -->
						{#each initialBoxes as box, index}
							{@const isLastBoxInRow = index % days.length === days.length - 1}
							{@const isLastBoxInColumn = Math.floor(index / days.length) === choreRows - 1}
							<div
								data-last-cell-in-row={isLastBoxInRow}
								data-last-cell-in-column={isLastBoxInColumn}
								style={`grid-template-columns: repeat(${days.length}, 1fr);`}
								class="flex flex-col items-center justify-center border-r border-b border-neutral-500 px-2 text-xs data-[last-cell-in-column='true']:border-b-0 data-[last-cell-in-row='true']:border-r-0"
							></div>
						{/each}
					</div>
				</div>
			</div>
		</AspectRatio>
	</div>
</main>
