<template>
<div id="app" class="container">
    <div id="row">
        <app-game-board></app-game-board>
    </div>
</div>
</template>

<script>
    import GameBoard from './components/GameBoard.vue'

    export default {
        data() {
            return {

            }
        },
        components: {
            appGameBoard: GameBoard
        }
    }
</script>

<style scoped>

</style>
