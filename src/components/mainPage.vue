<template>
    <!-- 2048 -->
    <div>
        <div class="header">
            <div class="name">2048</div>
            <div class="score">Score: {{ score }}</div>
        </div>
        <div>
            <div class="grid">
                <div class="grid-cell" v-for="cell in cells" :key="cell.id" :class="cell.class" :style="cell.style">
                    <div class="grid-cell-content">{{ cell.value }}</div>
                </div>
            </div>
        </div>
        <div class="footer">
            <div class="restart" @click="restart">Restart</div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'mainPage',
    data() {
        return {
            score: 0,
            cells: [],
        }
    },
    
    mounted() {
        this.init()
    },

    watch: {
        // keyborad event

        cells: {
            handler() {
                this.checkGameOver()
            },
            deep: true,
        },

    },
    methods: {
        init(){
            this.score = 0
            this.cells = []
            for (let i = 0; i < 16; i++) {
                this.cells.push({
                    id: i,
                    class: 'grid-cell',
                    style: {
                        top: `${Math.floor(i / 4) * 100}px`,
                        left: `${(i % 4) * 100}px`,
                    },
                })
            }
            this.addRandomCell()
            this.addRandomCell()
        },
        addRandomCell(){
            const emptyCells = this.cells.filter(cell => !cell.value)
            if (emptyCells.length) {
                const randomCell = emptyCells[Math.floor(Math.random() * emptyCells.length)]
                randomCell.value = Math.random() > 0.5 ? 2 : 4
                randomCell.class = 'grid-cell grid-cell-new'
            }
        },
        restart(){
            this.init()
        },
        checkGameOver(){
            const emptyCells = this.cells.filter(cell => !cell.value)
            if (emptyCells.length) {
                return
            }
            for (let i = 0; i < 16; i++) {
                const cell = this.cells[i]
                const leftCell = this.cells[i - 1]
                const rightCell = this.cells[i + 1]
                const topCell = this.cells[i - 4]
                const bottomCell = this.cells[i + 4]
                if (leftCell && leftCell.value === cell.value) {
                    return
                }
                if (rightCell && rightCell.value === cell.value) {
                    return
                }
                if (topCell && topCell.value === cell.value) {
                    return
                }
                if (bottomCell && bottomCell.value === cell.value) {
                    return
                }
            }
            
        },
        gameOver(){
            alert('Game Over' + this.score)
        },
        getKeysBoard(){
            document.addEventListener('keydown', (e) => {
                switch (e.key) {
                    case 'ArrowUp':
                        this.moveUp()
                        break
                    case 'ArrowDown':
                        this.moveDown()
                        break
                    case 'ArrowLeft':
                        this.moveLeft()
                        break
                    case 'ArrowRight':
                        this.moveRight()
                        break
                }
            })
        },
    },
}

</script>

<style scoped>
.header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0 20px;
    background-color: #eee4da;
    border-radius: 6px;
    box-shadow: 0 0 20px rgba(0, 0, 0, 0.15);
    margin-bottom: 20px;
}
.grid{
    position: relative;
    width: 100%;
    height: 100%;
    background-color: #bbada0;
    border-radius: 6px;
    box-shadow: 0 0 20px rgba(0, 0, 0, 0.15);
}
.grid-cell{
    position: absolute;
    width: 25%;
    height: 25%;
    border-radius: 6px;
    background-color: #cdc1b4;
    font-size: 50px;
    color: #776e65;
    line-height: 90px;
    text-align: center;
    box-shadow: 0 0 20px rgba(0, 0, 0, 0.15);
}
.grid-cell-content{
    position: absolute;
    width: 90px;
    height: 90px;
    border-radius: 6px;
    background-color: #cdc1b4;
    font-size: 50px;
    color: #776e65;
    line-height: 90px;
    text-align: center;
    box-shadow: 0 0 20px rgba(0, 0, 0, 0.15);
}
</style>
