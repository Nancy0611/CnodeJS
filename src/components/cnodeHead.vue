<template>
  <div class="head">
    <router-link to="/">
      <img src="../assets/cnodejs.svg" title="cnodejs.svg">
    </router-link>
    <el-button type='text' @click='dialogVisible=true'>关于</el-button>
    <el-dialog title='Vue.js重写CnodeJS社区'  :visible='dialogVisible'  :before-close="handleClose">
      <div class='dialogDiv'>
        <p>作者:&nbsp;&nbsp;<a href='https://nancy0611.github.io/' target='_blank'>nancy</a></p>
        <p>源代码:&nbsp;&nbsp;<a href='https://github.com/Nancy0611/CnodeJS' target='_blank'>Github</a></p>
        <p>技术栈:</p>
        <ul>
          <li>Vue.js</li>
          <li>Vue-router</li>
          <li>Vue-resource</li>
          <li>Element</li>
          <li>CnodeJs API</li>
        </ul>
      </div>
    </el-dialog>
  </div>
</template>

<script>
import ElButton from '../../node_modules/element-ui/packages/button/src/button.vue'

export default {
  data () {
    return {
      dialogVisible: false
    }
  },
  components: {ElButton},
  methods: {
    handleClose (done) {
      this.dialogVisible = false
    }
  }
}
</script>

<style scoped>
  .head{
    background: #324057;
    height: 3.6rem;
    width:100%;
    position: absolute;
    top:0;
    left: 0;
  }
  .head img{
    width: 10rem;
    height: 3.6rem;
    margin-left: 10rem;
  }
  .head>button{
    display: inline-block;
    float: right;
    line-height: 3.6rem;
    margin-right: 10rem;
    font-weight: bold;
    font-size: 20px;
    color: white;
    letter-spacing: 2px;
    padding-top: 0.2rem;
  }
  .head .dialogDiv{
    font-size: 17px;
  }
  .head ul{
    margin-left: 1rem;
  }
  .head a{
    text-decoration: none;
    color:#58b7ff
  }
</style>
