<script>
    import GridCell from "./GridCell.svelte";

    export let score;
    export let gridSize = 20;
    export let snake;
    export let direction;
    export let gem;
</script>

<style>
    .grid-container {
        width: 500px;
        margin: 0 auto;
    }

    .grid {
        box-shadow: 0 1px 1px rgba(0, 0, 0, 0.12),
        0 2px 2px rgba(0, 0, 0, 0.12),
        0 4px 4px rgba(0, 0, 0, 0.12),
        0 8px 8px rgba(0, 0, 0, 0.12),
        0 16px 16px rgba(0, 0, 0, 0.12);
    }

    .score {
        text-align: right;
        color: #EEEEEE;
    }

    .row {
        display: flex;
    }

</style>

<div class="grid-container">
    <div class="grid">
        {#each Array(gridSize) as _, i}
            <div class="row">
                {#each Array(gridSize) as _, j}
                    <GridCell bind:snake bind:direction bind:gem i={i} j={j}/>
                {/each}
            </div>
        {/each}
    </div>
    <h1 class="score">Score: {score}</h1>
</div>