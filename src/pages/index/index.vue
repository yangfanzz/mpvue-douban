<template>
  <div class="container1">
    <h1>正在热映</h1>
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
</template>

<script>
import store from '../../utils/store'

export default {
  data() {
    return {
      motto: '经典电影',
      userInfo: {},
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
  onError(err) {
    console.log(err)
  },
  onReachBottom(){
    if (this.nomore) {
      wx.showToast({
        title:'没有更多了哈~',
        icon:"none"
      })
      return
    }else{
      this.getData()
    }
  },
  methods: {
    getUserInfo() {
      // 调用登录接口
      wx.login({
        success: () => {
          wx.getUserInfo({
            success: (res) => {
              this.userInfo = res.userInfo
            }
          })
        }
      })
    },
    getData() {
      wx.showLoading({
        title:'加载中哈~'
      })
      wx.request({
        url: 'https://douban.uieee.com/v2/movie/in_theaters',
        header: {
          'content-type': 'json'
        },
        data:{
          start:this.start
        },
        success: (data) => {
          wx.hideLoading()
          var count=data.data.count
          var start=data.data.start
          var total=data.data.total
          this.total=data.data.total
          if(this.start==0){
            this.movies=data.data.subjects;
          }else{
            this.movies=this.movies.concat(data.data.subjects)
          }
          if (total<=count+start) {
            this.nomore=true
          }
          this.start=data.data.count
          // console.log(data);
        }
      })
    }
  },

  created() {
    this.getUserInfo()
    this.getData()
    // 调用应用实例的方法获取全局数据
  },
  mounted() {

  }
}
</script>

<style scoped >
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
