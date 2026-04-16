<script>
  let { children } = $props();
</script>

<div class="card">
  {@render children()}
</div>

<style>
  .card {
    min-height: 180px;
    padding: 20px;
    border-radius: 20px;

    background: rgba(255, 255, 255, 0.05);
    backdrop-filter: blur(16px);

    border: 1px solid rgba(255,255,255,0.08);

    transition: all 0.25s ease;
    position: relative;
    overflow: hidden;

    transition: transform 0.25s ease, box-shadow 0.25s ease;
  }

  .card::before {
    content: "";
    position: absolute;
    inset: 0;
    background: linear-gradient(120deg, transparent, rgba(124,58,237,0.2), transparent);
    opacity: 0;
    transition: 0.3s;
    pointer-events: none;
  }

  .card:hover::before {
    opacity: 1;
  }

  .card:hover {
    transform: translateY(-6px) scale(1.01);
    box-shadow: 0 20px 40px rgba(124, 58, 237, 0.25);
  }
  
  .card:active {
	transform: scale(1.03);
}
</style>