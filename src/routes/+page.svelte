<script>
	let canvas;

	$effect(() => {
		const ctx = canvas.getContext('2d');

		let ballX = canvas.width / 2;
		let ballY = canvas.height / 2;
		let ballDX = -2;
		let ballDY = -2;
		let ballRadius = 10;
		const paddleHeight = 10;
		const paddleWidth = 75;
		let paddleX = (canvas.width - paddleWidth) / 2;
		let rightPressed = $state(false);
		let leftPressed = $state(false);
		let score = 0;
		let lives = 3;

		const brickRowCount = 3;
		const brickColumnCount = 5;
		const brickWidth = 75;
		const brickHeight = 20;
		const brickPadding = 10;
		const brickOffsetTop = 30;
		const brickOffsetLeft = 30;

		const bricks = [];

		for (let c = 0; c < brickColumnCount; c++) {
			bricks[c] = [];
			for (let r = 0; r < brickRowCount; r++) {
				bricks[c][r] = { x: 0, y: 0, status: 1 };
			}
		}

		function drawBricks() {
			for (let c = 0; c < brickColumnCount; c++) {
				for (let r = 0; r < brickRowCount; r++) {
					if (bricks[c][r].status === 1) {
						const brickX = c * (brickWidth + brickPadding) + brickOffsetLeft;
						const brickY = r * (brickHeight + brickPadding) + brickOffsetTop;
						bricks[c][r].x = brickX;
						bricks[c][r].y = brickY;
						ctx.beginPath();
						ctx.rect(brickX, brickY, brickWidth, brickHeight);
						ctx.fillStyle = '#0095DD';
						ctx.fill();
						ctx.closePath();
					}
				}
			}
		}

		document.addEventListener('keydown', keyDownHandler);
		document.addEventListener('keyup', keyUpHandler);

		function keyDownHandler(e) {
			if (e.key === 'ArrowRight') {
				rightPressed = true;
			} else if (e.key === 'ArrowLeft') {
				leftPressed = true;
			}
		}

		function keyUpHandler(e) {
			if (e.key === 'ArrowRight') {
				rightPressed = false;
			} else if (e.key === 'ArrowLeft') {
				leftPressed = false;
			}
		}

		function collisionDetection() {
			for (let c = 0; c < brickColumnCount; c++) {
				for (let r = 0; r < brickRowCount; r++) {
					const b = bricks[c][r];
					if (b.status === 1) {
						if (
							ballX > b.x &&
							ballX < b.x + brickWidth &&
							ballY > b.y &&
							ballY < b.y + brickHeight
						) {
							ballDY = -ballDY;
							b.status = 0;
							score++;
							if (score === brickRowCount * brickColumnCount) {
								alert('YOU WIN, CONGRATULATIONS!');
							}
						}
					}
				}
			}
		}

		function drawScore() {
			ctx.font = '16px Arial';
			ctx.fillStyle = '#0095DD';
			ctx.fillText(`Score: ${score}`, 8, 20);
		}

		function drawLives() {
			ctx.font = '16px Arial';
			ctx.fillStyle = '#0095DD';
			ctx.fillText(`Lives: ${lives}`, canvas.width - 65, 20);
		}

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
			drawBricks();
			collisionDetection();
			drawScore();
			drawLives();

			if (ballY < ballRadius) {
				ballDY = -ballDY;
			} else if (ballY > canvas.height - ballRadius - paddleHeight) {
				if (ballX > paddleX && ballX < paddleX + paddleWidth) {
					ballDY = -ballDY;
				} else {
					lives--;
					if (!lives) {
						alert('GAME OVER');
						document.location.reload();
					} else {
						ballX = canvas.width / 2;
						ballY = canvas.height - 30;
						ballDX = 2;
						ballDY = -2;
						paddleX = (canvas.width - paddleWidth) / 2;
					}
				}
			}

			if (ballX > canvas.width - ballRadius || ballX < ballRadius) {
				ballDX = -ballDX;
			}

			ballX += ballDX;
			ballY += ballDY;

			if (rightPressed) {
				paddleX = Math.min(canvas.width - paddleWidth, paddleX + 7);
			} else if (leftPressed) {
				paddleX = Math.max(0, paddleX - 7);
			}

			requestAnimationFrame(draw);
		}

		draw();
	});
</script>

<canvas bind:this={canvas} width="600" height="600"></canvas>
