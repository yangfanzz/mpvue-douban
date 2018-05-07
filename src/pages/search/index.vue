<template>
  <div class="counter-warp">
    <div class="input-box" v-if="!searchSuccess">
      <input type="text" confirm-type="search" class="search-button" v-model="searchText">
      <i class="search-icon" @click="search"></i>
    </div>
    <div class="container1" v-else>
      <h1>搜索结果</h1>
      <section class="grid">
        <div v-if="movies.length>1" v-for="movie in movies" class="item">
          <div class="cover">
            <a :href="'/pages/detail/main?id='+movie.id">
              <img :src="movie.images.small">
            </a>
          </div>
          <div class="info">
            <h3>{{movie.title}}</h3>
            <p class="rank">
              {{movie.rating.average}}
            </p>
          </div>
        </div>
      </section>
    </div>
  </div>
</template>

<script>

export default {
  data() {
    return {
      searchText: '',
      searchSuccess:false,
      movies: [{
        images:{small:null},
        title:null,
        rating:{
          average:0
        }
      }],
      total:0,
      start:0,
      nomore:false
    }
  },
  methods: {
    search(){
      wx.showLoading({
        title:'加载中哈~'
      })
      wx.request({
        url: 'https://douban.uieee.com/v2/movie/search',
        header: {
          'content-type': 'json'
        },
        data:{
          q:this.searchText
        },
        success: (data) => {
          this.searchSuccess=true
          wx.hideLoading()
          // var count=data.data.count
          // var start=data.data.start
          // var total=data.data.total
          // this.total=data.data.total
          // if(this.start==0){
            this.movies=data.data.subjects;
          // }else{
          //   this.movies=this.movies.concat(data.data.subjects)
          // }
          // if (total<=count+start) {
          //   this.nomore=true
          // }
          // this.start=data.data.count
          // console.log(data);
        }
      })
    }
  }
}

</script>
<style>
.counter-warp {
  text-align: center;
  margin-top: 0px;
}
.input-box{
  width: 80%;
  height: 40px;
  margin: 100px auto;
  position: relative;
}
.search-button {
  display: inline-block;
  color: blue;
  border: 1px solid blue;
  border-radius: 10px;
  width: 100%;
  height: 100%;
}
.search-icon{
  position: absolute;
  right: 10px;
  top: 5px;
  display: inline-block;
  height: 30px;
  width: 30px;
  background: url('http://longyuan.oss-cn-qingdao.aliyuncs.com/ld/ai/pc/test/search.png') no-repeat;
  background-size: contain;
  z-index: 2;
}

.container1 {
  padding: 0 2%;
}

h1 {
  font-size: 24px;
  font-weight: normal;
  box-sizing: border-box;
  margin-left: auto;
  margin-right: auto;
  margin-bottom: 6px;
  padding-left: 4%;
}

.grid {
  padding: 20px 0;
  margin-left: auto;
  margin-right: auto;
  max-width: 660px;
  overflow: hidden;
  box-sizing: border-box;
}

.item {
  float: left;
  box-sizing: border-box;
  width: 33.33333%;
  padding-left: 4%;
  padding-right: 4%;
  margin-bottom: 20px;
  overflow: hidden;
}

.cover {
  height: 130px;
  overflow: hidden;
  position: relative;
  width: 100%;
  border-radius: 5px;
}
.cover a{
  display: inline-block;
  width: 100%;
  height: 100%;
}

.item img {
  width:100%;
  height:100%;
}
h3{
  font-size: 13px;
  font-weight: normal;
  padding: 5px 0 0;
  color: #494949;
  text-align: center;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 1;
  -webkit-box-orient: vertical;
}
.rank{
  height: 16px;
  line-height: 16px;
  font-size: 14px;
  background: #06aa28;
  color: #fff;
  border-radius: 5px;
  text-align: center;
}
</style>
