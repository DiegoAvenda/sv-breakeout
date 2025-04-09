<script>
	let canvas;

	$effect(() => {
		const ctx = canvas.getContext('2d');
		ctx.strokeRect(0, 0, canvas.width, canvas.height);
	});
</script>

<h1>hi</h1>

<canvas bind:this={canvas}></canvas>
