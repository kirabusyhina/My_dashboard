<script>
	let { tasks = [] } = $props();

	let total = $derived(tasks.length);
	let done = $derived(tasks.filter(t => t.done).length);
	let percent = $derived(total ? Math.round((done / total) * 100) : 0);
</script>

<div class="stats">
	<h3>📊 Progress</h3>

	<div class="numbers">
		<span>{done}/{total}</span>
		<span>{percent}%</span>
	</div>

	<div class="bar">
		<div class="fill" style="width: {percent}%"></div>
	</div>
</div>

<style>
	.stats {
		display: flex;
		flex-direction: column;
		gap: 12px;
	}

	.numbers {
		display: flex;
		justify-content: space-between;
		font-size: 14px;
		opacity: 0.8;
	}

	.bar {
		height: 10px;
		border-radius: 999px;
		background: rgba(255,255,255,0.1);
		overflow: hidden;
	}

	.fill {
		height: 100%;
		background: linear-gradient(90deg, #7c3aed, #f595f0);
		transition: width 0.3s ease;
	}
</style>