<script>
    export let gridSize;
    export let snakeHeadPosX;
    export let snakeHeadPosY;
</script>

<style>
    .grid {
        width: 500px;
        margin: 0 auto;
    }

    .row {
        display: flex;
    }

    .cell {
        background-color: #F0D9FF;
        height: 50px;
        width: 50px;
        border: 1px solid black;
    }

    .snake {
        background-color: #BFA2DB;
    }
</style>

<div class="grid">
    {snakeHeadPosX} {snakeHeadPosY}
    {#each Array(gridSize) as _, i}
        <div class="row">
            {#each Array(gridSize) as _, j}
                <div class="cell {snakeHeadPosX === j && snakeHeadPosY === i ? 'snake' : ''}"></div>
            {/each}
        </div>
    {/each}
</div>