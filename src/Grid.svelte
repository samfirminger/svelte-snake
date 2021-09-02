<script>
    import GridCell from "./GridCell.svelte";

    export let score;
    export let gridSize;
    export let snake;
    export let direction;
    export let gemType;
    export let gemPosX;
    export let gemPosY;


</script>

<style>
    .grid {
        width: 500px;
        margin: 0 auto;
    }

    .score {
        text-align: right;
        color: #EEEEEE;
    }

    .row {
        display: flex;
    }

</style>

<div class="grid">
    <h1 class="score">Score: {score}</h1>
    {#each Array(gridSize) as _, i}
        <div class="row">
            {#each Array(gridSize) as _, j}
                <GridCell snake="{snake}" direction="{direction}" foodType="{gemType}" foodPosX="{gemPosX}" foodPosY="{gemPosY}" i="{i}" j="{j}"/>
            {/each}
        </div>
    {/each}
</div>