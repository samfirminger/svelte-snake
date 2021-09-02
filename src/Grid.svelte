<script>
    export let score;
    export let gridSize;
    export let snake;
    export let foodPosX;
    export let foodPosY;

    function getCellName(snake, foodX, foodY, i, j) {
        if (snake.some(value => value[0] ===j && value[1] === i)) {
            return 'snake';
        }
        if (foodX === j && foodY === i) {
            return 'food'
        }
    }
</script>

<style>
    .grid {
        width: 750px;
        margin: 0 auto;
    }

    .score {
        text-align: right;
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

    .food {
        background-color: #CDF0EA;
    }
</style>

<div class="grid">
    <h1 class="score">Score: {score}</h1>
    {#each Array(gridSize) as _, i}
        <div class="row">
            {#each Array(gridSize) as _, j}
                <div class="cell {getCellName(snake, foodPosX, foodPosY, i, j)}"></div>
            {/each}
        </div>
    {/each}
</div>