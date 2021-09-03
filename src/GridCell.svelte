<script>
    export let snake;
    export let i;
    export let j;
    export let direction;
    export let foodType;
    export let foodPosX;
    export let foodPosY;

    function getCellName(snake, foodX, foodY, i, j) {
        let className = 'cell';
        if (snake.some(snakeCell => snakeCell[0] === j && snakeCell[1] === i)) {
            className += snake[0][0] === j && snake[0][1] === i ? ` snake-${direction}` : ' snake'
        } else if (foodX === j && foodY === i) {
            className += (' ' + foodType);
        }
        return className;
    }
</script>

<style>

    .cell {
        background-color: rgba(0,0,0,0.6);
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
    <div class="{getCellName(snake, foodPosX, foodPosY, i, j)}"></div>
</div>