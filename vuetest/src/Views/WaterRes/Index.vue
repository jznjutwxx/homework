<template>
    <div id="app">
        <el-row :gutter="15" class="row_bg">
            <el-col v-bind:xs="6" v-bind:sm="7" v-bind:md="7" v-bind:lg="7" v-bind:xl="6" class="sidescss">
                <div class="title" style="text-align:center;">月用水量(1月)</div>
                <comleft ref="szyleft"></comleft>
            </el-col>
            <el-col v-bind:xs="12" v-bind:sm="10" v-bind:md="10" v-bind:lg="10" v-bind:xl="12">
                <commap ref="comSzyMap"></commap>
            </el-col>
            <el-col v-bind:xs="6" v-bind:sm="7" v-bind:md="7" v-bind:lg="7" v-bind:xl="6" class="sidescss" >
                <div class="title ysxl">用水效率(2017年上半年)</div>
                <comright></comright>
            </el-col>
        </el-row>
    </div>
</template>
<script>
import comleft from './comleft'
import commap from './commap'
import comright from './comright'
export default {
    data(){
        return{

        }
    },
    components:{
        comleft:comleft,
        commap:commap,
        comright:comright
    },
    mounted(){
        
    },
    methods:{
        
    }
}
</script>
<style scoped>
    html,body,#app{
        width: 100%;
        height: 100%;
        margin: 0;
        padding: 0;
        background-color: #051266;
    }
    body{
        overflow: hidden;
    }
    #app{
        min-width: 1000px;
        min-height: 700px;
        overflow-y: auto;
        overflow-x: hidden;
    }
    .row_bg{
        margin: 15px 0 0 0;
    }
    #app .sidescss {
        position: relative;
    }
    #app .title {
        position: absolute;
        top: -10px;
        left: 35px;
        z-index: 1400;
        background-color: #051266;
        width: 120px;
        height: 20px;
        color: #22eff8;
        font-size: 16px;
        font-family: "microsoft yahei";
    }
</style>

