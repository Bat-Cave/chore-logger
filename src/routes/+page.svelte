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
			'Clear Out Old Food in Fridge',
			'Sweep Floor',
			'Mop Floor',
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
	const daysToShow = 24;
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
				<div class="grid grow grid-cols-11 grid-rows-9">
					<div
						class="col-span-2 row-span-1 flex flex-col items-center justify-center border-r-2 p-2"
					>
						<h1 class="text-2xl font-bold text-violet-700">Chore Logger</h1>
						<p class="text-center text-xs opacity-75">
							Initial the chores you do on the date you do them
						</p>
					</div>
					<div
						style={`grid-template-columns: repeat(${days.length}, 1fr);`}
						class="col-span-9 row-span-1 grid w-full max-w-full"
					>
						<!-- Dates -->
						{#each days as day}
							<div
								class="relative flex w-full items-center justify-center border-r border-violet-500 last:border-r-0"
							>
								<div
									class="absolute inset-0 flex flex-col items-center justify-center py-3 text-xs text-nowrap"
								>
									<p class="mb-auto text-xs font-semibold uppercase opacity-50">
										{format(day, 'MMM')}
									</p>
									<p class="text-sm font-semibold text-violet-700 uppercase">
										{format(day, 'ccccc')}
									</p>
									<p class="text-xl font-semibold">
										{format(day, 'd')}
									</p>
								</div>
							</div>
						{/each}
					</div>
					<div
						style={`grid-template-rows: repeat(${choreRows}, 1fr);`}
						class="col-span-2 row-span-8 grid border-t-2 border-r-2"
					>
						<!-- Chores -->
						{#each Object.entries(chores) as [room, roomchores], index}
							<div class="flex items-center justify-center bg-violet-500 text-white">
								<h2 class="text-lg font-semibold tracking-wider uppercase">{room}</h2>
							</div>
							{#each roomchores as chore, choreIndex}
								<div
									data-row-even={(choreIndex + index - 1) % 2 === 0}
									class="flex items-center border-b border-violet-500 px-2 last:border-b-0 data-[row-even='true']:bg-violet-100"
								>
									<p class="w-full text-right text-xs font-semibold">{chore}</p>
								</div>
							{/each}
						{/each}
					</div>
					<div
						style={`grid-template-rows: repeat(${choreRows}, 1fr); grid-template-columns: repeat(${days.length}, 1fr);`}
						class="col-span-9 row-span-8 grid w-full border-t-2"
					>
						<!-- Initial boxes -->
						{#each initialBoxes as box, index}
							{@const row = Math.floor(index / days.length)}
							{@const col = index % days.length}
							{@const isLastBoxInRow = col === days.length - 1}
							{@const isLastBoxInColumn = row === choreRows - 1}
							<div
								data-last-cell-in-row={isLastBoxInRow}
								data-last-cell-in-column={isLastBoxInColumn}
								data-row-even={row % 2 === 0}
								style={`grid-template-columns: repeat(${days.length}, 1fr);`}
								class="flex flex-col items-center justify-center border-r border-b border-violet-500 px-2 text-xs data-[last-cell-in-column='true']:border-b-0 data-[last-cell-in-row='true']:border-r-0 data-[row-even='true']:bg-violet-100"
							></div>
						{/each}
					</div>
				</div>
			</div>
		</AspectRatio>
	</div>
</main>
