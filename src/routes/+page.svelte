<script>
	let canvas;

	$effect(() => {
		const ctx = canvas.getContext('2d');

		let ballX = canvas.width / 2;
		let ballY = canvas.height / 2;
		let ballDX = 2;
		let ballDY = -2;
		let ballRadius = 10;
		const paddleHeight = 10;
		const paddleWidth = 75;
		let paddleX = (canvas.width - paddleWidth) / 2;

		function drawBall() {
			ctx.beginPath();
			ctx.arc(ballX, ballY, ballRadius, 0, Math.PI * 2);
			ctx.fillStyle = 'red';
			ctx.fill();
			ctx.closePath();
		}

		function drawPaddle() {
			ctx.beginPath();
			ctx.rect(paddleX, canvas.height - paddleHeight, paddleWidth, paddleHeight);
			ctx.fillStyle = 'blue';
			ctx.fill();
		}

		function draw() {
			ctx.clearRect(0, 0, canvas.width, canvas.height);

			ctx.strokeRect(0, 0, canvas.width, canvas.height);
			drawBall();
			drawPaddle();

			if (ballY < ballRadius) {
				ballDY = -ballDY;
			}

			if (ballX > canvas.width - ballRadius || ballX < ballRadius) {
				ballDX = -ballDX;
			}

			ballX += ballDX;
			ballY += ballDY;

			requestAnimationFrame(draw);
		}

		draw();
	});
</script>

<h1>hi</h1>

<canvas bind:this={canvas}></canvas>
