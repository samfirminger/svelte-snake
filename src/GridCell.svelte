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
        height: 50px;
        width: 50px;
        border: 1px solid black;
    }

    .snake {
        background-color: #4CAF50;
    }

    .snake-up {
        background-image: url(/images/snakeUp.png);
    }

    .snake-right {
        background-image: url(/images/snakeRight.png);
    }

    .snake-down {
        background-image: url(/images/snakeDown.png);
    }

    .snake-left {
        background-image: url(/images/snakeLeft.png);
    }

    .ruby {
        background-image: url(/images/ruby.png);
    }

    .emerald {
        background-image: url(/images/emerald.png);
    }

    .sapphire {
        background-image: url(/images/sapphire.png);
    }

    .wallUp-1 {
        background-image: url(/images/wall/wallUp-1.png);
    }
</style>

<div>
    <div class="{getCellName(snake, foodPosX, foodPosY, i, j)}"></div>
</div>