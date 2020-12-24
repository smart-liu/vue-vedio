<template>
    <div>
        <table>
            <thead>
                <tr class="cellTitle">
                    <!--  第一行是表头   -->
                    <th v-for="key in thList" v-bind:key="key">{{key}}</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(item, index) in keyValueList" v-bind:key="index">
                    <td v-for="k in item" v-bind:key="k.key">{{k.value}}</td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
    export default {
        props: ["thList","keyValueList"],
    }
</script>

<style lang="scss" scoped>
    table{
        display: block;
        overflow: auto;
        white-space: nowrap;
        height: 600px;
        tbody{
            /*tr:nth-child(odd){*/
            /*    background: #f0f0f0;*/
            /*}*/
            /*tr:nth-child(event){*/
            /*    background: white;*/
            /*}*/
            tr:hover{
                background: paleturquoise;
            }
        }
    }
    th{
        background: #cccccc;
        border-right: 1px rgba(0, 0, 0, 0.2) solid;
        text-align: center;
        height: 40px;
        line-height: 40px;
        white-space: nowrap;
    }
    tr{
        background: #f0f0f0;
        border-right: 1px rgba(0, 0, 0, 0.2) solid;
        text-align: center;
        height: 40px;
        line-height: 40px;
    }
    td{
        text-overflow: ellipsis;
    }
</style>
