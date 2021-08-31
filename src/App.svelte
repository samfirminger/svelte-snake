<script>
    import Grid from './Grid.svelte';
    import {onMount} from "svelte";

    const gridSize = 10;
    let snakeHeadPosX = Math.floor(Math.random() * 9);
    let snakeHeadPosY = Math.floor(Math.random() * 9);

    const directions = ['up', 'right', 'down', 'left'];
    let direction = directions[Math.floor(Math.random() * directions.length)];

    onMount(async () => {
        setInterval(updateSnakePos, 400);
    });

    function updateSnakePos() {
        switch (direction) {
            case 'up':
                snakeHeadPosY = snakeHeadPosY - 1 >= 0 ? snakeHeadPosY - 1 : gridSize - 1;
                break;
            case 'right':
                snakeHeadPosX = snakeHeadPosX + 1 <= gridSize - 1 ? snakeHeadPosX + 1 : 0;
                break;
            case 'down':
                snakeHeadPosY = snakeHeadPosY + 1 < gridSize ? snakeHeadPosY + 1 : 0;
                break;
            case 'left' :
                snakeHeadPosX = snakeHeadPosX - 1 >= 0 ? snakeHeadPosX - 1 : gridSize - 1;
                break;
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
        <Grid gridSize={gridSize} bind:snakeHeadPosX bind:snakeHeadPosY/>
    </div>
</main>

