<script>
	import Card from '$lib/components/Card.svelte';
	import Todo from '$lib/components/Todo.svelte';
	import Notes from '$lib/components/Notes.svelte';
    import Clock from '$lib/components/Clock.svelte';

	import { dndzone } from 'svelte-dnd-action';

	let widgets = $state([
		{ id: 1, type: 'todo' },
		{ id: 2, type: 'notes' },
		{ id: 3, type: 'clock' },
		{ id: 4, type: 'widget' }
	]);

	function handleDnd(e) {
		widgets = e.detail.items;
	}
</script>

<main class="dashboard">
	<h1 class="title">Dashboard</h1>

	<div
		class="grid"
		use:dndzone={{
			items: widgets,
			flipDurationMs: 300
		}}
		onconsider={handleDnd}
		onfinalize={handleDnd}
	>
		{#each widgets as widget (widget.id)}
			<Card>
				{#if widget.type === 'todo'}
					<Todo />
				{:else if widget.type === 'notes'}
					<Notes />
				{:else if widget.type === 'clock'}
					<Clock />
				{:else}
					<h3>⚙️ Widget</h3>
				{/if}
			</Card>
		{/each}
	</div>
</main>

<style>
	.dashboard {
		min-height: 100vh;
		padding: 40px;
	}

	.title {
		font-size: 32px;
		font-weight: 600;
		margin-bottom: 24px;

		background: linear-gradient(90deg, #7c3aed, #06b6d4);
		background-clip: text;
		-webkit-background-clip: text;
		-webkit-text-fill-color: transparent;
	}

	.grid {
		display: grid;
		grid-template-columns: repeat(2, 1fr);
		gap: 24px;
	}

	.grid :global(.card) {
		cursor: grab;
	}

	.grid :global(.card:active) {
		cursor: grabbing;
	}
</style>
