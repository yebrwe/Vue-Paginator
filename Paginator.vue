<template>
    <div class="pagination">
        <div class="pagination__column">
            <span>Page to: </span>
            <input type="number" class="pagination__input" v-model="inputPage" @keypress.enter="goPage(inputPage)"/>
            <span> / {{totalPage}}</span>
        </div>
        <div class="pagination__column">
            <div class="pagination__item" @click="goPage(1)" v-if="curPage > 1">
                <button class="pagination__button"> <v-icon> mdi-chevron-double-left </v-icon> </button>
            </div>
            <div class="pagination__item" @click="prevPage" v-if="curPage > 1">
                <button class="pagination__button"> <v-icon> mdi-chevron-left </v-icon> </button>
            </div>
            <div class="pagination__item" :class="{'pagination__item--active': curPage == page}" v-for="page in pages" @click="curPage != page && goPage(page)">
                <span v-if="curPage == page">{{page}}</span>
                <button class="pagination__button" v-else>{{page}}</button>
            </div>
            <div class="pagination__item" @click="nextPage" v-if="curPage < totalPage">
                <button class="pagination__button"> <v-icon> mdi-chevron-right </v-icon> </button>
            </div>
            <div class="pagination__item" @click="goPage(totalPage)" v-if="curPage < totalPage">
                <button class="pagination__button"> <v-icon> mdi-chevron-double-right </v-icon> </button>
            </div>
        </div>
        <div class="pagination__column">
        </div>
        <div class="pagination__column">
        </div>
    </div>
</template>
<style scoped>
    .pagination {
        padding: 0 25px;
        margin: 25px 0;
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
    .pagination__column { 
        display: flex;
        margin-left: 15px;
    }
    .pagination__item {
        margin-right: 8px;
        font-size: 18px;
        color: rgb(0, 0, 0, 0.6);
        cursor: pointer;
        padding: 5px 14px;
        border-radius: 5px;
        box-shadow: 1px 1px 3px 0px rgb(0, 0, 0, 0.3);
        box-sizing: border-box;
    }
    .pagination__item--active {
        cursor: initial;
        background-color: royalblue;
        color: white;
    } 
    .pagination__button {
        outline: none;
    }
    .pagination__input {
        margin-left: 10px;
        width: 65px;
        border: 1px solid rgb(0,0,0,0.3);
        outline: none;
        -moz-appearance: textfield;
    }
    .pagination__input::-webkit-outer-spin-button,
    .pagination__input::-webkit-inner-spin-button {
        -webkit-appearance: none;
        margin: 0;
    }
</style>
<script>
export default {
    props: ['curPage', 'totalPage'],
    data () {
        return {
            inputPage: 1,
            blockPage: 5,
        }
    },
    computed: {
        pages() {
            const startBlock = Math.floor((this.curPage-1) / this.blockPage);
            const startPage = startBlock * this.blockPage + 1;
            console.log(startBlock, startPage)
            let endPage = (startBlock + 1) * this.blockPage;
            endPage = endPage >= this.totalPage ? this.totalPage : endPage;
            const arr = []
            for(let i = startPage; i <= endPage; i++) {
                arr.push(i)
            }
            return arr
        }
    },
    watch: {
        "inputPage": {
            handler() {
                if(this.inputPage < 1){
                    this.inputPage = 1
                } else if(this.inputPage > this.totalPage){
                    this.inputPage = this.totalPage
                }
            }
        }
    },
    methods: {
        goPage(page) {
            if(!page) return
            this.inputPage = page
            this.$emit('page', page)
        },
        prevPage() {
            const page = Number(this.curPage) - 1;
            this.goPage(page);
        },
        nextPage() {
            const page = Number(this.curPage) + 1;
            this.goPage(page);
        }
    }
}
</script>