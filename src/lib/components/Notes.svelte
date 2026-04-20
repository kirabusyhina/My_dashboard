<script>
	import { onMount } from 'svelte';

	let text = $state('');

	// завантаження
	onMount(() => {
		const saved = localStorage.getItem('notes');
		if (saved) text = saved;
	});

	// автозбереження
	$effect(() => {
		localStorage.setItem('notes', text);
	});
</script>

<div class="notes">
	<h3>📘 Notes</h3>

	<textarea placeholder="Write something..." bind:value={text}></textarea>
</div>

<style>
	.notes {
		display: flex;
		flex-direction: column;
		gap: 12px;
		height: 100%;
	}

	h3 {
		margin: 0;
	}

	textarea {
		flex: 1;
		resize: none;
		border-radius: 12px;
		padding: 12px;
		border: none;
		outline: none;

		background: rgba(255, 255, 255, 0.05);
		color: #0f172a;

		font-size: 14px;
		transition: all 0.2s ease;
	}

	textarea:focus {
		background: rgba(255, 255, 255, 0.08);
	}

	:global(body[data-theme='dark']) textarea {
		color: white;
		background: rgba(255, 255, 255, 0.08);
	}
	:global(body[data-theme='light']) textarea {
		color: #0f172a;
		background: rgba(0, 0, 0, 0.05);
	}
</style>
