<template>
<div class="box " ref="box" v-if="info">
  <img :src="info.images.medium" class="glass">
  <div class="clearfix">
    <div class="img-box">
      <img :src="info.images.medium">
    </div>
    <div class="info">
      <p class="cn-name single">{{info.title}}</p>
      <p class="eng-name single">{{info.original_title}}</p>
      <p class="wish-count">{{info.wish_count}} 人想看</p>
      <p class="single duration">{{info.durations[0]}}</p>
      <p class="single">{{genres}}</p>
      <p class="single">{{info.mainland_pubdate+' 大陆上映'}}</p>
      <span>我想看</span><span class="rate">我要评分</span>
    </div>
  </div>
  <section class="subject-intro">
    <h2>剧情简介</h2>
    <div class="bd" style="position: static;">
      <p>{{info.summary}}</p>
    </div>
  </section>
  <section class="" id="celebrities">
    <header>
      <h2>影人</h2>
    </header>
    <div class="section-content">
      <ul class="row items">
        <li class="item item__celebrity"  v-if="info.directors.length>0" v-for="director in info.directors">
          <img class="item-poster" :src="director.avatars.small" v-if="director.avatars">
          <img class="item-poster" :src="none" v-else>
          <span class="item-title name">{{director.name}}</span>
          <span class="item-title role">导演</span>
        </li>
        <li class="item item__celebrity" v-for="director in info.casts">
          <img class="item-poster" :src="director.avatars.small" v-if="director.avatars">
          <img class="item-poster" :src="none" v-else>
          <span class="item-title name">{{director.name}}</span>
          <span class="item-title role">演员</span>
        </li>
      </ul>
    </div>
  </section>
  <section class="tags">
    <header>
      <h2>标签</h2>
      <ul >
        <li v-for="tag in info.tags">{{tag}}</li>
      </ul>
    </header>
  </section>
  <section class="subject-pics">
    <header>
      <h2>剧照</h2>
      <div class="bd photo-list">
        <ul >
          <li v-for="photo in info.photos">
            <img :src="photo.image" mode="aspectFill">
          </li>
        </ul>
      </div>
    </header>
  </section>
  <section class="subject-comments">
    <h2>热门短评</h2>
    <div class="bd comment-list">
      <ul>
        <li v-for="review in info.popular_comments">
          <div class="desc">
            <img :src="review.author.avatar">
            <div class="user-info">
              <strong>{{review.author.name}}</strong>
              <div class="date">{{review.created_at}}</div>
            </div>
          </div>
          <p>{{review.content}}</p>
        </li>
      </ul>
    </div>
  </section>
</div>
</template>

<script>
export default {

  data() {
    return {
      id: 0,
      info: null,
      genres: null,
      none:"https://img1.doubanio.com/f/movie/b6dc761f5e4cf04032faa969826986efbecd54bb/pics/movie/movie_default_small.png"
    }
  },
  onLoad(options) {
    // console.log(this.$root.$mp.query)
    this.id = this.$root.$mp.query.id
    wx.showLoading({
      title: '加载中哈~'
    })
    wx.request({
      url: 'https://douban.uieee.com/v2/movie/subject/' + this.id,
      header: {
        'content-type': 'json'
      },
      success: (data) => {
        wx.hideLoading()
        this.info = data.data;
        this.genres = data.data.genres.join("/");
        console.log(data.data);
      }
    })
  },
  created() {
    // this.id = this.$root
  }
}
</script>

<style>
.clearfix {
  /* clear: both; */
  overflow: hidden;
}

.single {
  white-space: nowrap;
  text-overflow: ellipsis;
  overflow: hidden;
}

.box {
  margin-left: 20px;
  margin-right: 20px;
}

.glass {
  width: 100%;
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  filter: blur(14px);
  z-index: -1;
}

.img-box {
  width: 126px;
  height: 188px;
  display: inline-block;
  margin-right: 10px;
  float: left;
  margin-top: 20px;
}

.img-box img {
  width: 100%;
  height: 100%;
}

.info {
  display: inline-block;
  height: 192px;
  width: 199px;
  color: #fff;
  float: right;
  margin-top: 20px;
  font-size: 14px;
  padding-bottom: 5px;
}

.info span {
  margin-top: 20px;
  display: inline-block;
  width: 90px;
  height: 40px;
  line-height: 40px;
  border: 1px solid #999;
  color: #eaeaea;
  text-align: center;
  border-radius: 48px;
}

.info .rate {
  margin-left: 10px;
  color: #659c0d;
  border-color: #659c0d;
}

.wish-count {
  color: #ff7e00;
  font-size: 15px;
}

.cn-name {
  font-size: 22px;
  text-align: left;
}

.eng-name {
  font-size: 14px;
  text-align: left;
}

section {
  margin: 30px 0;
}

section h2 {
  color: #aaa;
  margin: 0 0 15px;
  font-size: 15px;
}
.subject-intro{
  margin-top: 40px;
}
.subject-intro p {
  font-size: 15px;
  color: #494949;
}

.items {
  font-size: 0;
  white-space: nowrap;
  overflow-x: auto;
  color: #111;
}
.item{
  vertical-align: top;
}
.item-poster {
  width: 100%;
  height: 107px;
  overflow: hidden;
  background-size: cover;
  background-position: center;
}
.item__celebrity {
  font-size: 14px;
  width: 75px;
  display: inline-block;
  margin-right: 14px;
}
.item__celebrity .item-title {
  display: -webkit-box;
  overflow: hidden;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 3;
  font-size: 14px;
  line-height: 1.5;
  white-space: normal;
  text-align: center;
  color: #494949;
  margin-top: 8px;
}

.item__celebrity .item-title.role {
  color: #aaa;
}
.tags ul li{
  display: inline-block;
  margin: 10px 10px 0 0;
  font-size: 15px;
  line-height: 28px;
  padding: 0 12px;
  border-radius: 28px;
  text-align: center;
  color: #494949;
  background: #f5f5f5;
}

.photo-list {
    position: relative;
    height: 120px;
    margin-left: -18px;
}
.photo-list ul {
    word-break: keep-all;
    white-space: nowrap;
    overflow: scroll;
    -webkit-overflow-scrolling: touch;
    width: auto;
    position: absolute;
    left: 0;
    right: -18px;
}
.photo-list ul li {
    margin-right: 10px;
    height: 120px;
    overflow: hidden;
    display: inline-block;
  }
  .photo-list ul li img {
    height: 100%;
    display: block;
}
.comment-list li .desc {
    font-size: 0;
    line-height: normal;
    margin: 0 0 11px;
    color: #494949;
    position: relative;
  }
  .comment-list li .desc img{
    width: 36px;
    height: 36px;
    border-radius: 50%;
    vertical-align: text-top;
    margin-right: 10px;
    float: left;
}
.comment-list li .desc .user-info {
    margin-left: 40px;
}
.comment-list li .desc strong {
    font-size: 15px;
    display: inline-block;
    vertical-align: text-top;
    margin-right: 6px;
}
.comment-list li .desc .date {
    margin-top: 6px;
    font-size: 12px;
    color: #aaa;
}
.comment-list li p {
    padding: 0 0 0 40px;
    line-height: 22px;
    color: #494949;
    font-size: 15px;
    margin-bottom: 10px;
}
</style>
