<template>
    <div class="pageSwitch">
        <span class="item pre" v-on:click="clickPre()">
            &lt;pre </span>
        <span v-bind:class="['item', {active: currentPage===index}]" v-for="index in pageShow" v-bind:key="index" v-on:click="clickItem(index)">{{index}}</span>
        <span class="item next" v-on:click="clickNext()"> next&gt; </span>
        <span class="item">跳转</span>
        <select class="selectItem" v-model="currentPage">
            <option v-for="num in totalPage" v-bind:value="num">{{num}}</option>
        </select>
        <span>共{{totalPage}}页/每页{{once}}条/共{{total}}条</span>
    </div>
</template>

<script>
    export default {
        // Math.floor(5.55) //向下取整 结果为5
        // Math.floor(5.99) //向下取整 结果为5
        // Math.ceil(5.21) //向上取整，结果为6
        // Math.ceil(5.88) //向上取整，结果为6
        // Math.round(5.78) //四舍五入 结果为6
        // Math.round(5.33) //结果为5
        // once一页显示多少条数据，total一共多少条数据, pageShowOnly底部只显示多少页
        props: ["once", "total"],
        data(){
            return {
                currentPage: 1,
                pageShow: [],
                totalPage: 0,
                pageShowOnly: 8,
                selectedPage: 0
            }
        },
        mounted() {
            // 计算总页数，向下取整数（+1）
            this.totalPage = Math.ceil(this.total/this.once)
            // 计算显示页数
            if (this.pageShowOnly >= this.totalPage) {
                this.pageShow = Array.from(new Array(this.totalPage+1).keys()).slice(1)
            }else{
                this.pageShow = Array.from(new Array(this.pageShowOnly+1).keys()).slice(1)
            }
        },
        updated(){
            this.totalPage = Math.ceil(this.total/this.once)
        },
        methods: {
            clickItem(index){
                this.currentPage = index
            },
            clickPre(){
                if (this.currentPage !== 1) {
                    this.currentPage -= 1
                }
            },
            clickNext(){
                if (this.currentPage !== this.totalPage) {
                    this.currentPage += 1
                }
            },
        },
        watch: {
            currentPage: function () {
                // 监听当前页的数据改变，改变底部显示的标签页范围
                let addPage = Math.floor(this.pageShowOnly/2)
                if (this.pageShow.indexOf(this.currentPage) === -1) {
                    if ((this.currentPage + addPage) > this.totalPage) {
                        //右边增加的页数超出总页数
                        this.pageShow = Array.from(new Array(this.totalPage+1).keys()).slice(this.totalPage-this.pageShowOnly+1)
                    }else if ((this.currentPage - addPage) < 1) {
                        // 左边增加的页数少于总页数
                        this.pageShow = Array.from(new Array(this.pageShowOnly+1).keys()).slice(1)
                    }else{
                        // 右边增加或者减少均在页数范围内
                        this.pageShow = Array.from(new Array(this.currentPage+addPage+1).keys()).slice(this.currentPage-addPage)
                    }
                }
            }
        }
    }
</script>
<style lang="scss" scoped>
    .pageSwitch{
        border: 1px rgba(0, 0, 0, 0.2) solid;
        margin-left: 10px;
        margin-right: 10px;
        text-align: center;
        height: 32px;
        line-height: 32px;
        padding-top: 10px;
        padding-bottom: 10px;
    }
    .item{
        margin-right: 8px;
        display: inline-block;
        /*width: 5px;*/
    }
    .item:hover{
        background: #60b0ff;
        cursor: pointer;
    }
    .active{
        color: #60b0ff;
    }
    .selectItem{
        margin-right: 8px;
        display: inline-block;
    }
</style>
