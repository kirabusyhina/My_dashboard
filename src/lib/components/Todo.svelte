<script>
	import { onMount } from 'svelte';
	let { tasks = $bindable([]) } = $props();

	let input = $state('');

	function addTask() {
		if (!input.trim()) return;

		tasks = [...tasks, { text: input, done: false }];
		input = '';
	}

	function toggleTask(index) {
		tasks = tasks.map((task, i) => (i === index ? { ...task, done: !task.done } : task));
	}

	function deleteTask(index) {
		tasks = tasks.filter((_, i) => i !== index);
	}

	// ЗАВАНТАЖЕННЯ при старті
	onMount(() => {
		const saved = localStorage.getItem('tasks');
		if (saved) tasks = JSON.parse(saved);
	});

	// ЗБЕРЕЖЕННЯ при зміні
	$effect(() => {
		localStorage.setItem('tasks', JSON.stringify(tasks));
	});
</script>

<div class="todo">
	<h3>📃 Todo</h3>

	<div class="input">
		<input
			placeholder="Add a task..."
			bind:value={input}
			onkeydown={(e) => e.key === 'Enter' && addTask()}
		/>
		<button onclick={addTask}>+</button>
	</div>

	<ul>
		{#each tasks as task, i (i)}
			<li class:done={task.done}>
				<button type="button" onclick={() => toggleTask(i)}>
					{task.text}
				</button>
				<button onclick={() => deleteTask(i)}>✕</button>
			</li>
		{/each}
	</ul>
</div>

<style>
	.todo {
		display: flex;
		flex-direction: column;
		gap: 12px;
	}

	h3 {
		margin: 0;
	}

	.input {
		display: flex;
		gap: 8px;
	}

	input {
		flex: 1;
		padding: 8px;
		border-radius: 8px;
		border: none;
		outline: none;
		background: rgba(255, 255, 255, 0.5);
		color: white;
		transition: all 0.2s ease;
	}

	button {
		background: linear-gradient(135deg, #7c3aed, #06b6d4);
		border: none;
		border-radius: 8px;
		padding: 8px 12px;
		color: white;
		cursor: pointer;
	}

	ul {
		list-style: none;
		padding: 0;
		margin: 0;
	}

	li {
		display: flex;
		justify-content: space-between;
		padding: 6px 0;
		transition: all 0.2s ease;
	}

	.done button {
		text-decoration: line-through;
		opacity: 0.5;
		background: none;
		border: none;
		color: inherit;
		cursor: pointer;
		padding: 0;
		font: inherit;
		text-align: left;
	}
</style>
