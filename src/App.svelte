<script>
    import Grid from './Grid.svelte';
    import {onMount} from "svelte";

    const gridSize = 12;
    let snakeHeadPosX = Math.floor(Math.random() * (gridSize - 1));
    let snakeHeadPosY = Math.floor(Math.random() * (gridSize - 1));
    let snake = [[snakeHeadPosX, snakeHeadPosY]];
    let foodType;
    let foodPosX;
    let foodPosY;
    let score = 0;
    let gameOver = false;

    const directions = ['up', 'right', 'down', 'left'];
    let direction = directions[Math.floor(Math.random() * directions.length)];

    function generateNewFoodPos() {

        const x = Math.floor(Math.random() * (gridSize - 1));
        const y = Math.floor(Math.random() * (gridSize - 1));

        if (!snake.some(snakeCell => arrayEquals(snakeCell, [x, y]))) {
            foodPosX = x;
            foodPosY = y;

            let foodChance = Math.random();
            foodType = foodChance <= 0.3 ? 'emerald' : foodChance > 0.3 && foodChance < 0.6 ? 'sapphire' : 'ruby';
            return;
        }
        generateNewFoodPos();
    }

    onMount(async () => {
        generateNewFoodPos();
        setInterval(updateSnakePos, 200);
    });

    function updateSnakePos() {
        if (!gameOver) {
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

    }

    function arrayEquals(a, b) {
        return Array.isArray(a) &&
            Array.isArray(b) &&
            a.length === b.length &&
            a.every((val, index) => val === b[index]);
    }

    function moveSnake(position) {
        snake.unshift(position);
        snake = snake;

        if(snake.slice(1).some(position => arrayEquals(snake[0], position))) {
            gameOver = true;
            return;
        }

        snake.pop();
        snake = snake;
    }

    function checkForCollisions() {
        if (snake[0][0] === foodPosX && snake[0][1] === foodPosY) {
            snake.push([foodPosX, foodPosY]);
            generateNewFoodPos();
            score+=5;
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
        height: 100vh;
    }
</style>

<svelte:window on:keydown={handleKeydown}/>

<main>
    <div className="main-app-wrapper">
        {#if gameOver}
            <div>GAME OVER</div>
        {:else}
            <Grid gridSize={gridSize} bind:score bind:snake bind:direction bind:foodType bind:foodPosX bind:foodPosY/>
        {/if}
    </div>
</main>

