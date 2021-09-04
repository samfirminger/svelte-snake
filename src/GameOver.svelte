<script>
    export let score;
    let highScores = JSON.parse(localStorage.getItem('highScores')) || [];
    let currentScoreIndex = highScores.findIndex(highScore => highScore === score);

    function playAgain() {

    }
</script>

<style>
    .game-over {
        width: 30%;
        margin-left: auto;
        margin-right: auto;
        padding: 20px;
        background-color: rgba(20,22,47,0.5);
        border-radius: 5px;
        box-shadow: 0 1px 1px rgba(0,0,0,0.12),
        0 2px 2px rgba(0,0,0,0.12),
        0 4px 4px rgba(0,0,0,0.12),
        0 8px 8px rgba(0,0,0,0.12),
        0 16px 16px rgba(0,0,0,0.12);
    }

    .game-over-text {
        margin-bottom: 20px;
    }

    .game-score {
        margin-top: 10px;
    }

    .red {
        color: red;
    }

    .play-again {
        margin-top: 50px;
    }

    .play-again-button {
        padding: 10px;
        cursor: pointer;
    }

</style>

<div class="game-over">
    <h1 class="game-over-text">GAME OVER!</h1>
    <h2 class="game-over-text">{currentScoreIndex === 0 ? 'NEW HIGH SCORE: ' : 'You scored '}<span class="red">{score}</span></h2>
    <div>Your high scores</div>
    {#each highScores as score, i}
        <div class="game-score {i === currentScoreIndex ? ' red' : ''}">{i + 1}. {score}</div>
    {/each}
    <div class="play-again">
        <h1>Play again?</h1>
        <button class="play-again-button" on:click={playAgain}>YES</button>
    </div>
</div>