<template>
<el-tree :data="menus" :props="defaultProps" @node-click="handleNodeClick">
    <span class="custom-tree-node" slot-scope="{ node, data }">
        <span>{{ node.label }}</span>
        <span>
          <el-button
            type="text"
            size="mini"
            @click="() => append(data)">
            Append
          </el-button>
          <el-button
            type="text"
            size="mini"
            @click="() => remove(node, data)">
            Delete
          </el-button>
        </span>
      </span>
</el-tree>


</template>

<script>
//这里可以导入其他文件（比如：组件，工具 js，第三方插件 js，json 文件，图片文件等等）
//例如：import 《组件名称》 from '《组件路径》';

export default {
//import 引入的组件需要注入到对象中才能使用
data() {
      return {
        menus: [],
        defaultProps: {
          children: 'child',
          label: 'name'
        }
      };
    },
    methods: {
      handleNodeClick(data) {
        console.log(data);
      },
      getMenus(){
          this.$http({
          url: this.$http.adornUrl('/product/category/list/tree'),
          method: 'get',
        }).then(({data}) => {
            console.log("数据获取成功。。。"+data.data);
            this.menus=data.data;
          
          })
          
      },
      append(data) {
        
      },

      remove(node, data) {
          var ids=[data.catId]
        this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(()=>{
        this.$http({
        url: this.$http.adornUrl('product/category/delete'),
        method: 'post',
        data: this.$http.adornData(ids, false)
        }).then(({ data }) => {
            console.log("删除成功");
            this.getMenus;
         });

        })
  
      }

    },
components: {},
props: {},
data() {
//这里存放数据
return {

};
},
//计算属性 类似于 data 概念
computed: {},
//监控 data 中的数据变化
watch: {},
//方法集合
methods: {

},
//生命周期 - 创建完成（可以访问当前 this 实例）
created() {
    this.getMenus();
},
//生命周期 - 挂载完成（可以访问 DOM 元素）
mounted() {

},
beforeCreate() {}, //生命周期 - 创建之前
beforeMount() {}, //生命周期 - 挂载之前
beforeUpdate() {}, //生命周期 - 更新之前
updated() {}, //生命周期 - 更新之后
beforeDestroy() {}, //生命周期 - 销毁之前
destroyed() {}, //生命周期 - 销毁完成
activated() {}, //如果页面有 keep-alive 缓存功能，这个函数会触发 
}
</script>
<style lang=>
</style>