<script lang="ts">
	import type { KnowledgePanel } from '$lib/api';

	import Elements from './KnowledgeElements.svelte';
	import Card from '$lib/ui/Card.svelte';

	export let allPanels: Record<string, KnowledgePanel>;
	export let panel: KnowledgePanel | null;
	export let id: string;

	let expanded = panel?.expanded ?? false;
</script>

<div {id}>
	{#if panel == null}
		<div class="alert alert-warning">Panel is null</div>
	{:else if panel.type === 'card'}
		<Card>
			<h2 class="text-4xl font-bold my-3">{panel.title_element.title}</h2>

			<Elements elements={panel.elements} {allPanels} />
		</Card>
	{:else if panel.type === 'inline'}
		{#if panel.elements != null}
			<div class="pl-4">
				<Elements elements={panel.elements} {allPanels} />
			</div>
		{/if}
	{:else}
		<details
			bind:open={expanded}
			class:border-l-secondary={expanded}
			class:border-l-2={expanded}
			class:pl-2={expanded}
		>
			<summary
				class="dark:hover:bg-base-100 hover:bg-base-200 w-full rounded-lg p-2 my-2 cursor-pointer flex items-center select-none"
			>
				{#if panel.title_element != null}
					{#if panel.title_element.icon_url != null}
						{#if panel.title_element.type === 'grade'}
							<img
								class="h-12 mr-4"
								src={panel.title_element.icon_url}
								alt={panel.title_element.title}
							/>
						{:else}
							<img
								class="w-8 h-8 rounded-full mr-4 dark:invert"

								src={panel.title_element.icon_url}
								alt={panel.title_element.title}
							/>
						{/if}
					{/if}
					<span class="text-xl grow">
						{panel.title_element.title}
					</span>
				{/if}
			</summary>
			{#if panel.elements != null}
				<div class="pl-4">
					<Elements elements={panel.elements} {allPanels} />
				</div>
			{/if}
		</details>
	{/if}
</div>
