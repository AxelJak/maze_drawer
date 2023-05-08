
<script>
	import Button from '../lib/Button.svelte';
	import { onMount } from 'svelte';

	/**
     * @type {HTMLCanvasElement}
     */
	let canvas;

	let maze_color = "#FFF";
	let maze_number = 7;

	const width = 704;
	const height = 704;
	const cellSize = 22;


	let maze_map;

	function clearCanvas() {
		const cols = Math.floor(width / cellSize);
		const rows = Math.floor(height / cellSize);

		const context = canvas.getContext('2d');
		context.fillStyle = '#000';
		context.strokeStyle = '#ccc';
		context.lineWidth = 1;
		for (let i = 0; i < rows; i++) {
			for (let j = 0; j < cols; j++) {
				maze_map[i][j] = 0;
				const x = j * cellSize;
				const y = i * cellSize;
				context.fillRect(x + 1, y + 1, cellSize - 2, cellSize - 2);
				context.strokeRect(x, y, cellSize, cellSize);
			}
		}
		generateMaze();
	}

	onMount(() => {
		const cols = Math.floor(width / cellSize);
		const rows = Math.floor(height / cellSize);

		maze_map = new Array(cols).fill(0).map(() => new Array(rows).fill(0));

		const context = canvas.getContext('2d');
		
		context.fillStyle = '#000';
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
		generateMaze();

		canvas.addEventListener('mousedown', (event) => {
		const x = Math.floor(event.offsetX / cellSize);
		const y = Math.floor(event.offsetY / cellSize);

		// Update the state of the clicked cell in the grid
		maze_map[y][x] = maze_number;

		// Redraw the clicked cell on the canvas
		
		context.fillStyle = maze_color;
		context.fillRect((x * cellSize) + 1, (y * cellSize) + 1, cellSize - 2, cellSize - 2);
		

		// Listen for mousemove event while the mouse button is held down
		canvas.addEventListener('mousemove', onMouseMove);

		function onMouseMove(event) {
			const x = Math.floor(event.offsetX / cellSize);
			const y = Math.floor(event.offsetY / cellSize);

			// Update the state of the cell in the grid
			maze_map[y][x] = maze_number;

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
			generateMaze();
		}
		});
	});

	function generateMaze() {
		let maze = document.getElementById("maze");
		maze.innerHTML = "{";

		for (let i = 0; i < 32; i++) {
			if (i == 31){
				maze.innerHTML += "{" + maze_map[i] + "}}";
			} else {
				maze.innerHTML += "{" + maze_map[i] + "},<br>";
			}
		}
	}

</script>


<div id="colorButtons">
	<button on:click={clearCanvas} id="clear">Clear</button>
	<button on:click={() => { maze_color = '#201FFF'; maze_number = 2;}} id="blue">Blue</button>
	<button on:click={() => { maze_color = '#000'; maze_number = 0; }} id="black">Black</button>
	<button on:click={() => { maze_color = '#FFF'; maze_number = 7; }} id="white">White</button>
	<button on:click={() => { maze_color = '#FF0000'; maze_number = 1; }} id="red">Red</button>
	<button on:click={() => { maze_color = '#2ECC71'; maze_number = 4; }} id="green">Green</button>
	<button on:click={() => { maze_color = '#fcba03'; maze_number = 5; }} id="yellow">Yellow</button>
</div>

<div id="maze_div">
	<canvas
		bind:this={canvas}
		width={width}
		height={height}
	></canvas>

	<p id="maze"></p>

</div>




<style>

	#maze_div {
		display: grid;
		grid-template-columns: 1fr 1fr;
		justify-items: center;
	}

	canvas {
		border: #000 solid;
		width: 704px;
		height: 704px;
		background-color: #666;
	}

	p{
		border: #000 solid;
		word-break: break-all;
		white-space: normal;
		text-align: center;
		font-family: monospace;
		font-size: large;

		background-color: #ECEFF1;
		border-radius: 8px;
		padding: 2em;
	}

	

	#colorButtons {
		width: 704px;
		display: flex;
		align-items: center;
		justify-content: center;
		margin-left: 5em;
	}
	button {
		border-radius: 8px;
		border: #575757 solid;
		border-width: 0.7px;
		box-sizing: border-box;
		color: #FFFFFF;
		cursor: pointer;
		display: inline-block;
		font-family: "Haas Grot Text R Web", "Helvetica Neue", Helvetica, Arial, sans-serif;
		font-size: 14px;
		font-weight: 500;
		height: 40px;
		line-height: 20px;
		list-style: none;
		margin: 10px;
		outline: none;
		padding: 10px 16px;
		position: relative;
		text-align: center;
		text-decoration: none;
		transition: color 100ms;
		vertical-align: baseline;
		user-select: none;
		-webkit-user-select: none;
		touch-action: manipulation;
	}

	#clear {
		background-color: #ccc;
	}
	#clear:hover,
	#clear:focus {
		background-color: #F3F2F2;
	}

	#black {
		background-color: #000;
	}
	#black:hover,
	#black:focus {
		background-color: #90A4AE;
	}

	#blue {
		background-color: #201FFF;
	}
	#blue:hover,
	#blue:focus {
		background-color: #2196F3;
	}

	#green {
		background-color: #2ECC71;
	}
	#green:hover,
	#green:focus {
		background-color: #82E0AA;
	}

	#red {
		background-color: #FF0000;
	}
	#red:hover,
	#red:focus {
		background-color: #FF6363;
	}
	#yellow {
		background-color: #fcba03;
	}
	#yellow:hover,
	#yellow:focus {
		background-color: #f7d165;
	}

	#white {
		background-color: #FFF;
		color: #000;
	}
	#white:hover,
	#white:focus {
		background-color: #F3F2F2;
	}



	

	
</style>
