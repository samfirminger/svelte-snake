<script>
    import { createEventDispatcher } from 'svelte';

    //Declare the dispatch
    const dispatch = createEventDispatcher();

    export let score;
    let highScores = JSON.parse(localStorage.getItem('highScores')) || [];
    let currentScoreIndex = highScores.findIndex(highScore => highScore === score);

    function playAgain() {
        dispatch('playAgain');
    }
</script>

<style>

    @media only screen and (min-width: 200px) {
        .game-over {
            width: 80%;
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
    }

    @media only screen and (min-width: 600px) {
        .game-over {
            width: 60%;
        }
    }

    @media only screen and (min-width: 768px) {
        .game-over {
            width: 30%;
        }
    }

    .game-over__text {
        margin-bottom: 20px;
    }

    .game-over__score {
        margin-top: 10px;
    }

    .red {
        color: red;
    }

    .game-over__play-again {
        margin-top: 50px;
    }

    .game-over__play-again-button {
        padding: 10px;
        cursor: pointer;
    }

    .game-over__play-again-button:hover {
        background-color: #b5b3b3;
    }

</style>

<div class="game-over">
    <h1 class="game-over__text">GAME OVER!</h1>
    <h2 class="game-over__text">{currentScoreIndex === 0 ? 'NEW HIGH SCORE: ' : 'You scored '}<span class="red">{score}</span></h2>
    <div>Your high scores</div>
    {#each highScores as score, i}
        <div class="game-over__score {i === currentScoreIndex ? ' red' : ''}">{i + 1}. {score}</div>
    {/each}
    <div class="game-over__play-again">
        <h1>Play again?</h1>
        <button class="game-over__play-again-button" on:click={playAgain}>YES</button>
    </div>
</div>