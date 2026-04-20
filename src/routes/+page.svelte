<script>
	import Card from '$lib/components/Card.svelte';
	import Todo from '$lib/components/Todo.svelte';
	import Notes from '$lib/components/Notes.svelte';
	import Clock from '$lib/components/Clock.svelte';
	import Stats from '$lib/components/Stats.svelte';

	let theme = $state('dark');

	$effect(() => {
		if (typeof window !== 'undefined') {
			const saved = localStorage.getItem('theme');
			if (saved) theme = saved;
		}
	});

	$effect(() => {
		if (typeof window !== 'undefined') {
			localStorage.setItem('theme', theme);
		}
	});

	$effect(() => {
		if (typeof document !== 'undefined') {
			document.body.dataset.theme = theme;
		}
	});

	let tasks = $state([]);

	import { dndzone } from 'svelte-dnd-action';

	let widgets = $state([
		{ id: 1, type: 'todo' },
		{ id: 2, type: 'notes' },
		{ id: 3, type: 'clock' },
		{ id: 4, type: 'stats' }
	]);

	$effect(() => {
		if (typeof window !== 'undefined') {
			const saved = localStorage.getItem('widgets');
			if (saved) {
				widgets = JSON.parse(saved);
			}
		}
	});
	$effect(() => {
		if (typeof window !== 'undefined') {
			localStorage.setItem('widgets', JSON.stringify(widgets));
		}
	});

	function handleDnd(e) {
		widgets = e.detail.items;
	}

	function toggleTheme() {
		theme = theme === 'dark' ? 'light' : 'dark';
	}
</script>

<main class="dashboard">
	<div class="header">
		<h1 class="title">Dashboard</h1>

		<button class="theme-btn" onclick={toggleTheme}>
			{theme === 'dark' ? '☀️' : '🌙'}
		</button>
	</div>

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
					<Todo bind:tasks />
				{:else if widget.type === 'notes'}
					<Notes />
				{:else if widget.type === 'clock'}
					<Clock />
				{:else if widget.type === 'stats'}
					<Stats {tasks} />
				{:else}
					<h3>⚙️ Widget</h3>
				{/if}
			</Card>
		{/each}
	</div>
</main>

<style>
	.dashboard {
		min-width: 1100px;
		margin: 0 auto;
		padding: 40px;
	}

	.title {
		font-size: 32px;
		font-weight: 600;
		margin-bottom: 24px;

		background: linear-gradient(90deg, #7c3aed, #f595f0);
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

	.header {
		display: flex;
		justify-content: space-between;
		align-items: center;
		margin-bottom: 24px;
	}

	.theme-btn {
		width: 40px;
		height: 40px;
		border-radius: 12px;
		border: none;
		cursor: pointer;

		background: rgba(255, 255, 255, 0.1);
		backdrop-filter: blur(10px);

		transition: all 0.2s ease;
	}

	.theme-btn:hover {
		transform: scale(1.05);
	}
</style>
