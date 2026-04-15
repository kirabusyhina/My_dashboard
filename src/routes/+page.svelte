<script>
	import Card from '$lib/components/Card.svelte';
	import Todo from '$lib/components/Todo.svelte';
	import Notes from '$lib/components/Notes.svelte';
</script>

<main class="dashboard">
	<h1 class="title">Dashboard</h1>

	<div class="grid">
		<Card>
			<Todo />
		</Card>

		<Card>
			<Notes />
		</Card>
        
		<Card>⏰ Clock</Card>
		<Card>⚙️ Widget</Card>
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
		grid-template-columns: 1fr 1fr;
		gap: 24px;
	}

	.grid :global(.wide) {
		grid-column: span 2;
	}
</style>
