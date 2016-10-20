<template>
  <div id="secondcomponent">
    <h1>I am another page</h1>
    <el-card class="box-card">
      <div slot="header" class="clearfix">
        <h1 style="line-height: 36px; color: #20A0FF">豆瓣电影排行榜</h2>
      </div>
      <div v-for="(article,index) in articles" class="text item">
        <div class="text item" v-show="(index>=pageSize*(currentPage-1))&&(index<pageSize*currentPage)?true:false">
          <a :href="article.alt">{{index}}-{{article.title}}</a>
        </div>
      </div>
      <div class="block">
    <span class="demonstration">显示总数</span>
    <el-pagination
      @sizechange="handleSizeChange"
      @currentchange="handleCurrentChange"
      :current-page="currentPage"
      :page-size="pageSize"
      layout="total, prev, pager, next"
      :total="list.length">
    </el-pagination>
  </div>
    </el-card>

  </div>
</template>

<script>

export default {

  data() {
    return {

      // articles: newList,
      articles:[],
      // count:'',
      list:[],
      pageSize:10,
      currentPage:1,
show:true

    }
  },
  mounted: function() {
    this.$http.jsonp('https://api.douban.com/v2/movie/top250?count=99', {}, {
        headers: {

        },
        emulateJSON: true
    }).then(function(response) {
      // 这里是处理正确的回调
      this.list=response.data.subjects
         this.articles=response.data.subjects
        //  this.count=list.length
console.log(this.articles);

        // this.articles = response.data["subjects"] 也可以

    }, function(response) {
        // 这里是处理错误的回调
        console.log(response)
    });
  },
  methods:{
    handleSizeChange(val) {
      this.pageSize=val;

          console.log(`每页 ${val} 条`);
        },
    handleCurrentChange(val) {
      this.currentPage=val;
    //  this.articles=this.list.slice(0,5)
          console.log(`当前页: ${val}`);
        }
  },
  // computed:{
  // show:function(){
  // return  (index>=(currentPage-1)*pageSize$$index<=currentPage*pageSize)
  // }
  // }

}
</script>

<style>

</style>
