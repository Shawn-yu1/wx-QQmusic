<template>
 <div class="wrapper">
    <swiper class="swiper" indicator-dots autoplay indicator-active-color="white" circular='true'>
      <swiper-item v-for="(item,index) in swiper" :key="index" @click="swiperUrl(item.id)">
        <img :src="item.picUrl" alt="图片">
      </swiper-item>
    </swiper>
    <div class="diantai">电台</div>
    <div class="hotbox">
      <div class="show" v-for="(item,index) in hotbox" :key="index">
        <img :src="item.picUrl" alt="">
        <text class="title">{{item.Ftitle}}</text>
      </div>
    </div>
    <div class="hot">热门歌单</div>
    <div class="songlist">
      <div class="list" v-for="(item,index) in songlist" :key="index">
        <img :src="item.picUrl" alt="">
        <p>{{item.songListDesc}}</p>
        <b>{{item.songListAuthor}}</b>
      </div>
    </div>
    <div class="ending">
      <p class="endtitle">查看电脑版网页</p>
      <img class="endlogo" src="//y.gtimg.cn/mediastyle/mod/mobile/img/logo_ch.svg?max_age=2592000" alt="">
      <p class="text">Copyright © 1998 -<span>2019</span> Tencent. All Rights Reserved. </p>
      <p class="phone">联系电话：0755-86013388 QQ群：55209235</p>
    </div>
    <div class="over">
      <div class="text">安装QQ音乐 发现更多精彩</div>
    </div>
 </div>
</template>

<script>
export default {
  data () {
    return {
      swiper:[],
      hotbox:[],
      songlist:[]
    }
  },
  methods: {
     swiperUrl(e){
        let seleted = this.swiper.find((item)=>{
          return item.id==e
        })
        let url = seleted.linkUrl; 
     }
  },
  created(){
      wx.request({
          url: 'https://c.y.qq.com/musichall/fcgi-bin/fcg_yqqhomepagerecommend.fcg?_=1554690676597&g_tk=5381&uin=0&format=json&inCharset=utf-8&outCharset=utf-8&notice=0&platform=h5&needNewCode=1', //开发者服务器接口地址",
          data: 'data', //请求的参数",
          method: 'GET',
          dataType: 'json', //如果设为json，会尝试对返回的数据做一次 JSON.parse
          success: res => {
              let msgbox = res.data.data; 
              this.swiper=msgbox.slider;
              this.hotbox=msgbox.radioList;
              this.songlist=msgbox.songList;
          },
          fail: () => {},
          complete: () => {}
        });
  }
}
</script>


<style scoped>
.wrapper{
  background:	#F0F0F0;
  font-size: 30rpx;
}
.index_top{
  width:100%;
  height:80rpx;
  display: flex;
  justify-content: space-around;
  background: #fff;
  align-items: center;
}
.logo{
  width:250rpx;
  height:70rpx;
}
.index_top a{
  height:50rpx;
  width:160rpx;
  border:1px solid black;
  border-radius: 30rpx;
  font-size: 30rpx;
  text-align: center;
}
.swiper{
  width:100%;
  height:400rpx;
}
.swiper img{
  width:100%;
  height:100%;
} 
.diantai{
  width:100%;
  height:80rpx;
  line-height: 80rpx;
  text-indent: 10rpx;
}
.hotbox{
  width:100%;
  height:400rpx;
  display: flex;
  justify-content: space-around;
}
.hotbox text{
   font-size: 30rpx;
}
.show{
  width:45%;
  height:100%;
  flex-direction: column;
  background:#fff;
}
.show img{
  width:100%;
  height:80%;
}
.hot{
  width:100%;
  height:80rpx;
  line-height: 80rpx;
  text-indent: 10rpx;
}
.songlist{
  width:100%;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
}
.list{
  width:45%;
  height:400rpx;
  margin-bottom: 20rpx;
  background: #fff;
}
.list img{
  width:100%;
  height:80%;
}
.list p{
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    font-size: 30rpx;
}

.list b{
  font-size: 20rpx;
  color:#ccc;
}
.ending{
  width:100%;
  height:200rpx;
  text-align: center;
}
.ending .endlogo{
  width:200rpx;
  height:50rpx;
}
.ending .endtitle{
  font-size:30rpx;
  margin:30rpx 0 ;
}
.ending .text{
  font-size: 20rpx;
  margin-top:20rpx;
}
.ending .phone{
  font-size: 20rpx;
  padding:10rpx 0;
}
.over{
  width:100%;
  height:100rpx;
  background: #fff;
  padding-top:30rpx;
}
.over .text{
  width:60%;
  height:80%;
  background: #30c27c;
  color: #fff;
  text-align: center;
  line-height: 80rpx;
  border-radius: 50rpx;
  margin:0 auto;

}
</style>