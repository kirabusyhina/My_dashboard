<script>
	import { onMount } from "svelte";

	let time = $state("");

	function updateTime() {
		const now = new Date();
		time = now.toLocaleTimeString();
	}

	onMount(() => {
		updateTime();
		const interval = setInterval(updateTime, 1000);

		return () => clearInterval(interval);
	});
</script>

<div class="clock">
	<h3>🕰️ Clock</h3>
	<div class="time">{time}</div>
</div>

<style>
	.clock {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		height: 100%;
		gap: 10px;
	}

	.time {
		font-size: 32px;
		font-weight: bold;
		background: linear-gradient(90deg, #7c3aed, #06b6d4);
		background-clip: text;
		-webkit-background-clip: text;
		-webkit-text-fill-color: transparent;
	}
</style>