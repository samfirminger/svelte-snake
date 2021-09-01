<script>
    import Grid from './Grid.svelte';
    import {onMount} from "svelte";

    const gridSize = 10;
    let snakeHeadPosX = Math.floor(Math.random() * (gridSize - 1));
    let snakeHeadPosY = Math.floor(Math.random() * (gridSize - 1));
    let snake = [[snakeHeadPosX, snakeHeadPosY]];
    let foodPosX;
    let foodPosY;

    const directions = ['up', 'right', 'down', 'left'];
    let direction = directions[Math.floor(Math.random() * directions.length)];

    function generateNewFoodPos() {

        const x = Math.floor(Math.random() * (gridSize - 1));
        const y = Math.floor(Math.random() * (gridSize - 1));

        if (x !== snakeHeadPosX && y !== snakeHeadPosY) {
            foodPosX = x;
            foodPosY = y;
            return;
        }
        generateNewFoodPos();
    }

    onMount(async () => {
        generateNewFoodPos();
        setInterval(updateSnakePos, 100);
    });

    function updateSnakePos() {
        checkForCollisions();
        switch (direction) {
            case 'up':
                moveSnake([snake[0][0], snake[0][1] - 1 >= 0 ? snake[0][1] - 1 : gridSize - 1]);
                break;
            case 'right':
                moveSnake([snake[0][0] + 1 <= gridSize - 1 ? snake[0][0] + 1 : 0, snake[0][1]]);
                break;
            case 'down':
                moveSnake([snake[0][0], snake[0][1] + 1 < gridSize ? snake[0][1] + 1 : 0]);
                break;
            case 'left' :
                moveSnake([snake[0][0] - 1 >= 0 ? snake[0][0] - 1 : gridSize - 1, snake[0][1]])
                break;
        }

    }

    function moveSnake(position) {
        snake.unshift(position);
        snake = snake;
        snake.pop();
        snake = snake;
    }

    function checkForCollisions() {
        if (snake[0][0] === foodPosX && snake[0][1] === foodPosY) {
            snake.push([foodPosX, foodPosY]);
            generateNewFoodPos();
        }
    }


    function handleKeydown(event) {
        event.preventDefault();
        switch (event.keyCode) {
            case 38 :
                direction = direction !== 'down' ? 'up' : direction;
                break;
            case 39 :
                direction = direction !== 'left' ? 'right' : direction;
                break;
            case 40 :
                direction = direction !== 'up' ? 'down' : direction;
                break;
            case 37:
                direction = direction !== 'right' ? 'left' : direction;
                break;
        }
    }
</script>

<style>
    main {
        text-align: center;
        padding: 1em;
        margin: 0 auto;
    }
</style>

<svelte:window on:keydown={handleKeydown}/>

<main>
    <div className="main-app-wrapper">
        {#if gameOver}
            <Grid gridSize={gridSize} bind:snake bind:foodPosX bind:foodPosY/>
            </div>
            </main>

