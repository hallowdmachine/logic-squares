<template lang="html">
    <div class="board">
        <div v-for="(row, rowIndex) in gameBoard">
            <div
                v-for="(square, squareIndex) in row"
                class="square"
                :class="{ selected: square }"
                @click="clickSquare(square, rowIndex, squareIndex)">
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data () {
            return {
                gameBoard: {
                    1: {
                        1: false,
                        2: false,
                        3: false
                    },
                    2: {
                        1: false,
                        2: true,
                        3: false
                    },
                    3: {
                        1: false,
                        2: false,
                        3: false
                    }
                },
                gameHasBegun: false
            }
        },

        created () {
            // console.log(this.gameBoard)
        },

        methods: {
            clickSquare (squareValue, rowIndex, squareIndex) {
                // parseInt the rowIndex and squareIndex to ensure the maths used
                // in the this.getAdjacent() works as it should
                let row = parseInt(rowIndex)
                let square = parseInt(squareIndex)

                // toggle the square
                this.toggle(row, square)

                // call this.getAdjacent and assign the returned value
                let adjacents = this.getAdjacent(row, square)

                let xA, xB, yL, yR, x, y

                for (let square in adjacents) {
                    if (adjacents.hasOwnProperty(square))
                        if (adjacents[square].hasOwnProperty('xA'))
                            xA = adjacents[square].xA
                        if (adjacents[square].hasOwnProperty('xB'))
                            xB = adjacents[square].xB
                        if (adjacents[square].hasOwnProperty('yL'))
                            yL = adjacents[square].yL
                        if (adjacents[square].hasOwnProperty('yR'))
                            yR = adjacents[square].yR
                        if (adjacents[square].hasOwnProperty('x'))
                            x = adjacents[square].x
                        if (adjacents[square].hasOwnProperty('y'))
                            y = adjacents[square].y
                }

                this.toggleAdjacents(row, square, xA, xB, yL, yR, x, y)
            },

            toggle (rowIndex, squareIndex) {
                this.gameBoard[rowIndex][squareIndex] = !this.gameBoard[rowIndex][squareIndex]
            },

            getAdjacent (x, y) {
                // subtract and add one to get the rows (x) and squares (y)
                // that are above (A) and below (B)
                let xA = x - 1
                let xB = x + 1
                let yL  = y - 1
                let yR = y + 1

                // if the values for x or y go out-of-bounds (that is, 0 or 4),
                // change to 1 or 3. this is to prevent trying to toggle non-existent
                // squares on the board.
                // i'll have to check the clicked square's coords to stop toggling
                // it again
                // TODO: this doesn't work. well, not really. need to return only
                // coords for the actual adjacent squares. doing it this way inevitably
                // returns the coords for the clicked square, except in the event of the
                // center square
                if (xA === 0)
                    xA = 1
                if (xB === 4)
                    xB = 3
                if (yL === 0)
                    yL = 1
                if (yR === 4)
                    yR = 3

                // return an object containing the squares above, below, to the left,
                // and to the right of the clicked square
                return {
                    top: {xA, y},
                    bottom: {xB, y},
                    left: {x, yL},
                    right: {x, yR}
                }
                // only the center square will return four unique squares, of course
                // the rest will have at least one set of coords equal to their own
            },

            toggleAdjacents (row, square, xA, xB, yL, yR, x, y) {
                // necessary because of the bungled logic in this.getAdjacent, but it works
                if (xA != row || y != square)
                    this.toggle(xA, y)
                if (xB != row || y != square)
                    this.toggle(xB, y)
                if (x != row || yL != square)
                    this.toggle(x, yL)
                if (x != row || yR != square)
                    this.toggle(x, yR)
            }
        }
    }
</script>

<style lang="css" scoped>
    .board {
        width: 300px;
    }

    .square {
        height: 100px;
        width: 100px;
        border: 1px solid rgb(0, 0, 0);
        float: left;
    }

    .selected {
        background-color: rgb(0, 0, 0);
    }
</style>
