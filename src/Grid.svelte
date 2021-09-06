<script>
    import GridCell from "./GridCell.svelte";

    export let score;
    export let gridSize = 20;
    export let snake;
    export let direction;
    export let gem;
</script>

<style>
    .grid {
        width: 500px;
        margin: 0 auto;
    }

    .grid__container {
        box-shadow: 0 1px 1px rgba(0, 0, 0, 0.12),
        0 2px 2px rgba(0, 0, 0, 0.12),
        0 4px 4px rgba(0, 0, 0, 0.12),
        0 8px 8px rgba(0, 0, 0, 0.12),
        0 16px 16px rgba(0, 0, 0, 0.12);
    }

    .grid__score {
        text-align: right;
        color: #EEEEEE;
    }

    .grid__row {
        display: flex;
    }

</style>

<div class="grid">
    <div class="grid__container">
        {#each Array(gridSize) as _, i}
            <div class="grid__row">
                {#each Array(gridSize) as _, j}
                    <GridCell bind:snake bind:direction bind:gem i={i} j={j}/>
                {/each}
            </div>
        {/each}
    </div>
    <h1 class="grid__score">Score: {score}</h1>
</div>