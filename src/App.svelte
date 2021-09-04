<script>
    import Grid from './Grid.svelte';
    import {onMount} from "svelte";
    import GameOver from "./GameOver.svelte";
    import {arrayEquals} from "./shared.js";

    // grid
    const gridSize = 20;
    // snake
    let snakeHeadPosX = 6;
    let snakeHeadPosY = 10;
    let initialSnake = [[snakeHeadPosX, snakeHeadPosY], [snakeHeadPosX - 1, snakeHeadPosY]];
    let snake = [...initialSnake];
    const directions = ['up', 'right', 'down', 'left'];
    let direction = directions[1];
    // gem
    const gems = {
        ['diamond']: 50,
        ['ruby']: 20,
        ['emerald']: 10,
        ['sapphire']: 5
    }
    let gem = {};
    // game info
    let score = 0;
    let gameOver = false;
    let interval;

    onMount(async () => {
        generateNewGemPos();
        interval = setInterval(updateSnakePos, 150);
    });

    function generateNewGemPos() {

        const x = Math.floor(Math.random() * (gridSize - 1));
        const y = Math.floor(Math.random() * (gridSize - 1));

        if (!snake.some(snakeCell => arrayEquals(snakeCell, [x, y]))) {
            gem.x = x;
            gem.y = y;

            let gemDieRoll = Math.random();
            gem.type = gemDieRoll <= 0.05 ? 'diamond' :
                gemDieRoll > 0.05 && gemDieRoll <= 0.2 ? 'ruby' :
                    gemDieRoll > 0.2 && gemDieRoll < 0.4 ? 'emerald' : 'sapphire';
            return;
        }
        generateNewGemPos();
    }


    function updateSnakePos() {

        if (!gameOver) {
            checkForGemCollision();
            switch (direction) {
                case 'up':
                    if (snake[0][1] - 1 < 0) {
                        triggerGameOver();
                        break;
                    }
                    moveSnake([snake[0][0], snake[0][1] - 1]);
                    break;
                case 'right':
                    if (snake[0][0] + 1 >= gridSize) {
                        triggerGameOver();
                        break;
                    }
                    moveSnake([snake[0][0] + 1, snake[0][1]]);
                    break;
                case 'down':
                    if (snake[0][1] + 1 >= gridSize) {
                        triggerGameOver();
                        break;
                    }
                    moveSnake([snake[0][0], snake[0][1] + 1]);
                    break;
                case 'left' :
                    if (snake[0][0] - 1 < 0) {
                        triggerGameOver();
                        break;
                    }
                    moveSnake([snake[0][0] - 1, snake[0][1]])
                    break;
            }
        }
    }

    function triggerGameOver() {
        setHighScore();
        gameOver = true;
        interval = clearInterval();
    }

    function moveSnake(position) {
        snake.unshift(position);
        snake = snake;

        if (snake.slice(1).some(snakeCell => arrayEquals(snake[0], snakeCell))) {
            triggerGameOver();
            return;
        }

        snake.pop();
        snake = snake;
    }

    function setHighScore() {
        if (score === 0) {
            return;
        }
        const highScores = JSON.parse(localStorage.getItem('highScores')) || [];
        if (!highScores.length || highScores.length < 10 || highScores.some(highScore => highScore <= score)) {
            highScores.push(score);
            highScores.sort((a, b) => b - a);
            localStorage.setItem('highScores', JSON.stringify(highScores.slice(0, 10)));
        }
    }

    function checkForGemCollision() {
        if (arrayEquals(snake[0], [gem.x, gem.y])) {
            snake.push([gem.x, gem.y]);
            score = score + gems[gem.type];
            generateNewGemPos();
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

    function playAgain() {
        snake = [...initialSnake];
        direction = directions[1];
        score = 0;
        generateNewGemPos();
        gameOver = false;
    }
</script>

<style>
    main {
        text-align: center;
        padding: 1em;
        margin: 0 auto;
        height: 100vh;

        height: 100vh;
        -webkit-flex-flow: column wrap;
        -ms-flex-flow: column wrap;
        flex-flow: column wrap;
        display: -webkit-box;
        display: -webkit-flex;
        display: -ms-flexbox;
        display: flex;
        -webkit-box-pack: center;
        -webkit-justify-content: center;
        -ms-flex-pack: center;
        justify-content: center;
    }

    .main-app-wrapper {
        display: flex;
        align-items: center;
    }
</style>

<svelte:window on:keydown={handleKeydown}/>

<main>
    <div class="main-app-wrapper">
        {#if gameOver}
            <GameOver score={score}  on:playAgain={playAgain}/>
        {:else}
            <Grid gridSize={gridSize} bind:score bind:snake bind:direction bind:gem/>
        {/if}
    </div>
</main>

