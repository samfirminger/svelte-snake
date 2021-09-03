<script>
    import Grid from './Grid.svelte';
    import {onMount} from "svelte";
    import GameOver from "./GameOver.svelte";

    const gridSize = 20;
    let snakeHeadPosX = 6;
    let snakeHeadPosY = 10;
    let snake = [[snakeHeadPosX, snakeHeadPosY], [snakeHeadPosX - 1, snakeHeadPosY]];
    let gemType;
    let gemPosX;
    let gemPosY;
    let score = 0;
    let gameOver = false;

    const directions = ['up', 'right', 'down', 'left'];
    let direction = directions[1];

    function generateNewGemPos() {

        const x = Math.floor(Math.random() * (gridSize - 1));
        const y = Math.floor(Math.random() * (gridSize - 1));

        if (!snake.some(snakeCell => arrayEquals(snakeCell, [x, y]))) {
            gemPosX = x;
            gemPosY = y;

            let gemDieRoll = Math.random();
            gemType = gemDieRoll <= 0.05 ? 'diamond' :
                gemDieRoll > 0.05 && gemDieRoll <= 0.3 ? 'emerald' :
                    gemDieRoll > 0.3 && gemDieRoll < 0.8 ? 'sapphire' : 'ruby';
            return;
        }
        generateNewGemPos();
    }

    onMount(async () => {
        generateNewGemPos();
        setInterval(updateSnakePos, 150);
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

        if (snake.slice(1).some(position => arrayEquals(snake[0], position))) {
            gameOver = true;
            setHighScore();
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

    function checkForCollisions() {
        if (snake[0][0] === gemPosX && snake[0][1] === gemPosY) {
            snake.push([gemPosX, gemPosY]);
            score = score + (gemType === 'diamond' ? 50 : gemType === 'ruby' ? 20 : gemType === 'emerald' ? 10 : 5);
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
</style>

<svelte:window on:keydown={handleKeydown}/>

<main>
    <div className="main-app-wrapper">
        {#if gameOver}
            <GameOver score={score}/>
        {:else}
            <Grid gridSize={gridSize} bind:score bind:snake bind:direction bind:gemType bind:gemPosX bind:gemPosY/>
        {/if}
    </div>
</main>

