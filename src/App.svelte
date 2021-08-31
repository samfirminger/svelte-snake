<script>
	import Grid from './Grid.svelte';
	const gridSize = 10;
	const snakeHeadPosX = Math.floor(Math.random() * 9);
	const snakeHeadPosY = Math.floor(Math.random() * 9);
</script>

<style>
	main {
		text-align: center;
		padding: 1em;
		margin: 0 auto;
	}
</style>

<main>
	<div className="main-app-wrapper">
		<Grid gridSize={gridSize} snakeHeadPosX={snakeHeadPosX} snakeHeadPosY={snakeHeadPosY}/>
	</div>
</main>

