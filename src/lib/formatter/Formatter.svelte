<script lang="ts">
	import ParserInput from '$lib/parser/ParserInput.svelte';
	import { onMount } from 'svelte';
	import { Formatter, rainterpreterOpMeta, type StateConfig } from '@rainprotocol/rainlang';

	export let readOnly: boolean = true;
	export let raw: string | null = null;
	export let stateConfig: StateConfig | null = null;

	let formatter: ParserInput;

	let showFormatter: boolean = true;

	onMount(() => {
		if (stateConfig) raw = Formatter.get(stateConfig, { opmeta: rainterpreterOpMeta });
		if (raw) formatter.loadRaw(raw.toLowerCase());
		if (!raw && !stateConfig) showFormatter = false;
	});
</script>

{#if showFormatter}
	<div class="bg-gray-100 dark:bg-gray-800 rounded-lg p-4">
		<ParserInput {readOnly} bind:this={formatter} />
	</div>
{/if}
