<script lang="ts">
	export let options: {
		value?: any;
		label: string;
	}[];
	export let exclusive: boolean = false;
	export let exclusiveOptions: number[] | null = null;
	export let defaultOption: number | null = null;
	export let value: any[] = [];

	let exclusiveValue: any;

	$: value = exclusive
		? exclusiveValue
		: options.filter((item, i) => filterStatuses[i]).map((item) => item?.value || item.label);

	let filterStatuses = options.map((filter) => false);

	const toggleFilter = (i: number) => {
		if (exclusive || exclusiveOptions?.includes(i)) filterStatuses = options.map((filter) => false);
		if (exclusive) {
			filterStatuses[i] = true;
		} else {
			filterStatuses[i] = !filterStatuses[i];
		}
		if (exclusiveOptions && !exclusiveOptions.includes(i))
			filterStatuses = filterStatuses.map((e, i) => (exclusiveOptions?.includes(i) ? false : e));
		exclusiveValue = options[i]?.value || options[i].label;
	};

	// set the default
	if (defaultOption) toggleFilter(0);
</script>

<div class="flex gap-2 flex-wrap">
	{#each options as option, i}
		<span
			class="cursor-pointer rounded-2xl py-2 px-3 transition-colors whitespace-nowrap"
			class:toggledOn={filterStatuses[i]}
			class:toggledOff={!filterStatuses[i]}
			on:click={() => {
				toggleFilter(i);
			}}
			>{option.label}
		</span>
	{/each}
</div>

<style lang="postcss" global>
	:local(.toggledOn) {
		@apply bg-gray-800 text-gray-100 dark:bg-gray-200 dark:text-black;
	}

	:local(.toggledOff) {
		@apply bg-gray-200 text-gray-800 hover:bg-gray-300 dark:bg-gray-800 dark:text-white;
	}
</style>
