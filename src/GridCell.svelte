<script>
    import {arrayEquals} from "./shared.js";

    export let snake;
    export let i = undefined;
    export let j = undefined;
    export let direction = undefined;
    export let gem = {};

    function getCellName(snake, gem, i, j) {
        let cellClassName = 'cell';

        if (snake.some(snakeCell => arrayEquals(snakeCell, [j, i]))) {
            cellClassName += arrayEquals(snake[0], [j,i]) ? ` snake-${direction}` : ' snake'
        } else if (gem.x === j && gem.y === i) {
            cellClassName += (' ' + gem.type);
        }
        return cellClassName;
    }

</script>

<style>

    .cell {
        background-color: rgba(0, 0, 0, 0.6);
        height: 25px;
        width: 25px;
    }

    .snake {
        background-color: #4CAF50;
    }

    .snake-up {
        background-image: url(/images/snakeFullUpSmall.png);
    }

    .snake-right {
        background-image: url(/images/snakeFullRightSmall.png);
    }

    .snake-down {
        background-image: url(/images/snakeFullDownSmall.png);
    }

    .snake-left {
        background-image: url(/images/snakeFullLeftSmall.png);
    }

    .ruby {
        background-image: url(/images/rubyT.png);
    }

    .emerald {
        background-image: url(/images/emeraldT.png);
    }

    .sapphire {
        background-image: url(/images/sapphireT.png);
    }

    .diamond {
        background-image: url(/images/diamondT.png);
    }

</style>

<div>
    <div class="{getCellName(snake, gem, i, j)}"></div>
</div>