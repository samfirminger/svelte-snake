<script>
    export let score;
    let highScores = JSON.parse(localStorage.getItem('highScores')) || [];
</script>

<style>
    .game-over {
        margin-top: 200px;
        width: 40%;
        margin-left: auto;
        margin-right: auto;
        padding: 20px;
        background-color: rgba(20,22,47,0.5);
    }

    .title {
        margin-bottom: 20px;
    }

    .game-score {
        margin-top: 10px;
    }

</style>

<div class="game-over">
    <div class="title">GAME OVER!</div>
    <div class="title">You scored {score}</div>
    <div>High scores</div>
    {#each highScores as score, i}
        <div class="game-score">{i + 1}. {score}</div>
    {/each}
</div>