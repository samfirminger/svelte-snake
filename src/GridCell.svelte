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
        background-color: black;
        height: 25px;
        width: 25px;
    }

    .roundedTopLeft {
        border-radius: 10px 0 0 0 ;
    }

    .snake {
        background-color: #4CAF50;
    }

    .snake-up {
        background-image: url(/images/snakeUpSmall.png);
    }

    .snake-right {
        background-image: url(/images/snakeRightSmall.png);
    }

    .snake-down {
        background-image: url(/images/snakeDownSmall.png);
    }

    .snake-left {
        background-image: url(/images/snakeLeftSmall.png);
    }

    .ruby {
        background-image: url(/images/rubySmall.png);
    }

    .emerald {
        background-image: url(/images/emeraldSmall.png);
    }

    .sapphire {
        background-image: url(/images/sapphireSmall.png);
    }

</style>

<div>
    <div class="{getCellName(snake, foodPosX, foodPosY, i, j)}"></div>
</div>