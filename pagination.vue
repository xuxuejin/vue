<template>
    <div class="pagination">
        <a href="javascript:;" class="previous" :class="{previousDisable: currentPage == 1 ? true : false}" @click="go(currentPage - 1)"></a>
        <span class="page-num">
            <!-- <a href="javascript:;" class="active">1</a> -->
            <!-- <a href="javascript:;">2</a> -->
            <a :class="{active: currentPage == 1 ? true : false}" @click="go(1)">1</a>
            <strong v-show="pages[0] > 2">...</strong>
            <a v-for="(page, index) in pages" :key="index" :class="{active: currentPage == page ? true : false}" @click="go(page)">{{page}}</a>
            <strong v-show="pages[pages.length-1] < totalPage - 1">...</strong>
            <a v-if="totalPage > 1" :class="{active: currentPage == totalPage ? true : false}" @click="go(totalPage)">{{totalPage}}</a>
        </span>
        <a href="javascript:;" class="next" :class="{nextDisable: currentPage >= totalPage ? true : false}" v-show="currentPage < totalPage && showNext" @click="go(currentPage + 1)"></a>
        <span v-if="showJump" v-show="totalPage > 1">跳至<input type="number" min="1" :max="totalPage" v-model="jumpPage" class="jump-num" @keyup.enter="go(jumpPage)">页</span>
    </div>
</template>
<script>
export default {
    data() {
        return {
            currentPage: 0,
            jumpPage: 0
        }
    },
    props: {
        totalPage: { // 总页数
            type: Number,
            default: 1,
            required: true
        },
        showItems: { // 显示出来的页数，如: 1 ... 34[5]67 ... 10
            type: Number,
            default: 5
        },
        showPrev: { // 是否显示“上一页”
            type: Boolean,
            default: true
        },
        showNext: { // 是否显示“下一页”
            type: Boolean,
            default: true
        },
        showJump: { // 是否显示“跳转”
            type: Boolean,
            default: true
        },
        initPage: {
            type: Number,
            default: 1
        }
    },
    computed:{
        pages () {
            let counts = this.showItems
            // 根据起始页码和结束页码得到页码数组
            let getPages = (start,end) => {
                if(start <= 1 || start > end || start >= this.totalPage) {
                    start = 2
                }
                if(end >= this.totalPage || end < start || end <= 1) {
                    end = this.totalPage - 1
                }
                let arr = []
                for(let i = start; i <= end; i++) {
                    arr.push(i)
                }
                return arr
            }

            if(this.totalPage < counts + 2) {
                return getPages(2,this.totalPage)
            } else {
                if(this.currentPage <= Math.ceil(counts/2)) {
                    return getPages(2,counts)
                } else if(this.currentPage >= this.totalPage - Math.floor(counts/2)) {
                    return getPages(this.totalPage + 1 - counts,this.totalPage - 1)
                } else {
                    let half = Math.ceil(counts/2) - 1
                    let end = this.currentPage + half
                    if(counts % 2 === 0) {
                        end++
                    }
                    return getPages(this.currentPage - half,end)
                }
            }
        }
    },
    created() {
        this.currentPage = this.initPage;
    },
    methods:{
        go (page) {

            if(page < 1) page = 1;

            if(page > this.totalPage) page = this.totalPage;

            if(page === this.currentPage) return;

            this.currentPage = parseInt(page,10)
            
            // 发送页数
            this.$emit('goPage', this.currentPage)
        }
    },
    watch: {
        currentPage (newVal) {
            this.jumpPage = newVal
        },
        initPage (newVal) {
            if(this.currentPage !== newVal) {
                this.currentPage = newVal
            }
        }
    }
}
</script>
<style lang="less" scoped>
    .pagination {
        display: table;
        margin: 40px auto 20px;
        width: auto;
        height: 32px;
        overflow: hidden;
        line-height: 32px;
        font-size: 14px;
        color: rgba(0, 0, 0, .65);
        .page-num,
        strong {
            float: left;
        }
        .previous,
        .next {
            position: relative;
            &:before {
                position: absolute;
                top: 10px;
                left: 10px;
                content: '';
                width: 12px;
                height: 12px;
            }
            &:hover {
                border-color: #2576A5;
                background-color: #ffffff;
            }
        }
        .previous {
            &:before {
                background: url(../assets/img/common/previous-icon.png) center center no-repeat;
                background-size: contain;
            }
            &:hover {
                &:before {
                    background: url(../assets/img/common/previous-hover-icon.png) center center no-repeat;
                    background-size: contain;
                }
            }
        }
        .previousDisable {
            cursor: no-drop;
            &:before {
                background: url(../assets/img/common/previous-disable-icon.png) center center no-repeat;
                background-size: contain;
            }
            &:hover {
                border-color: rgba(0, 0, 0, .15);
                &:before {
                    background: url(../assets/img/common/previous-disable-icon.png) center center no-repeat;
                    background-size: contain;
                }
            }
        }
        .next {
            &:before {
                background: url(../assets/img/common/next-icon.png) center center no-repeat;
                background-size: contain;
            }
            &:hover {
                &:before {
                    background: url(../assets/img/common/next-hover-icon.png) center center no-repeat;
                    background-size: contain;
                }
            }
        }
        .nextDisable {
            cursor: no-drop;
            &:before {
                background: url(../assets/img/common/next-disable-icon.png) center center no-repeat;
                background-size: contain;
            }
            &:hover {
                border-color: rgba(0, 0, 0, .15);
                &:before {
                    background: url(../assets/img/common/next-disable-icon.png) center center no-repeat;
                    background-size: contain;
                }
            }
        }
        a {
            float: left;
            margin-right: 8px;
            width: 30px;
            height: 30px;
            line-height: 32px;
            text-align: center;
            border-radius:4px;
            border:1px solid rgba(0, 0, 0, .15);
            background-color: #fff;
            color: rgba(0, 0, 0, .65);
            cursor: pointer;
            &:hover {
                color: #2576A5;
                border-color: #2576A5;
            }
        }
        strong {
            margin-left: -8px;
            width: 32px;
            height: 32px;
            line-height: 26px;
            text-align: center;
        }
        .active {
            color: #fff;
            border-color: #2576A5;
            background-color: #2576A5;
            &:hover {
                color: #fff;
            }
        }
        .jump-num {
            margin: 0 8px;
            width: 60px;
            height: 30px;
            line-height: 32px;
            text-align: center;
            border-radius:4px;
            border:1px solid rgba(0, 0, 0, .15);
        }
    }

</style>


