

<script>
	import { onMount } from 'svelte';

	/**
     * @type {HTMLCanvasElement}
     */
	let canvas;

	let maze_color = "#000";

	function clearCanvas() {
		const width = canvas.width;
		const height = canvas.height;

		const cellSize = 20;
		const cols = Math.floor(width / cellSize);
		const rows = Math.floor(height / cellSize);


		const context = canvas.getContext('2d');
		// @ts-ignore
		context.fillStyle = '#ADADAD';
		context.strokeStyle = '#ccc';
		context.lineWidth = 1;
		for (let i = 0; i < rows; i++) {
			for (let j = 0; j < cols; j++) {
				const x = j * cellSize;
				const y = i * cellSize;
				context.fillRect(x + 1, y + 1, cellSize - 2, cellSize - 2);
				context.strokeRect(x, y, cellSize, cellSize);
			}
		}
	}

	onMount(() => {

		const width = canvas.width;
		const height = canvas.height;

		const cellSize = 20;
		const cols = Math.floor(width / cellSize);
		const rows = Math.floor(height / cellSize);

		const grid = new Array(cols).fill(null).map(() => new Array(rows).fill(null));

		const context = canvas.getContext('2d');
		
		context.fillStyle = '#ADADAD';
		context.strokeStyle = '#ccc';
		context.lineWidth = 1;
		for (let i = 0; i < rows; i++) {
			for (let j = 0; j < cols; j++) {
				const x = j * cellSize;
				const y = i * cellSize;
				context.fillRect(x + 1, y + 1, cellSize - 2, cellSize - 2);
				context.strokeRect(x, y, cellSize, cellSize);
			}
		}

		canvas.addEventListener('mousedown', (event) => {
		const x = Math.floor(event.offsetX / cellSize);
		const y = Math.floor(event.offsetY / cellSize);

		// Update the state of the clicked cell in the grid
		grid[y][x] = 1;

		// Redraw the clicked cell on the canvas
		
		context.fillStyle = maze_color;
		context.fillRect((x * cellSize) + 1, (y * cellSize) + 1, cellSize - 2, cellSize - 2);
		

		// Listen for mousemove event while the mouse button is held down
		canvas.addEventListener('mousemove', onMouseMove);

		function onMouseMove(event) {
			const x = Math.floor(event.offsetX / cellSize);
			const y = Math.floor(event.offsetY / cellSize);

			// Update the state of the cell in the grid
			grid[y][x] = 1;

			// Redraw the cell on the canvas
			context.fillStyle = maze_color;
			
			context.fillRect((x * cellSize) + 1, (y * cellSize) + 1, cellSize - 2, cellSize - 2);
		}

		// Listen for mouseup event to stop painting
		canvas.addEventListener('mouseup', onMouseUp);

		function onMouseUp() {
			// Stop listening for mousemove and mouseup events
			canvas.removeEventListener('mousemove', onMouseMove);
			canvas.removeEventListener('mouseup', onMouseUp);
		}
		});
	});
</script>

<div id="colorButtons">
	<button on:click={clearCanvas} id="blue">Clear</button>
	<button on:click={() => { maze_color = '#201FFF' }} id="blue">Blue</button>
	<button on:click={() => { maze_color = '#000' }} id="black">Black</button>
	<button on:click={() => { maze_color = '#FFF' }} id="white">White</button>
	<button on:click={() => { maze_color = '#FF0000' }} id="red">Red</button>
	<button on:click={() => { maze_color = '#329313' }} id="green">Green</button>
</div>

<canvas
	bind:this={canvas}
	width={640}
	height={640}
></canvas>




<style>
	canvas {
		width: 640px;
		height: 640px;
		background-color: #666;
	}

	#colorButtons {
		/*border: #000 solid;*/
		width: 640px;
		border-width: 1px;
		display: flex;
		align-items: center;
		justify-content: center;
		margin-block: 10px;
	}
	button {
		margin-inline: 10px;
	}
</style>
