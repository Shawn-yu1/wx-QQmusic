<template>
    <div>
        <div class="box">
            <img :src="data.mbHeadPicUrl" alt="">
            <p>{{data.title}}</p>
            <view>{{data.titleDetail}}</view>
            <text>更新时间：{{data.updateTime}}</text>
        </div>
        <div class="sort">排行榜：{{num}}</div>
        <div class="list">
            <div class="song" v-for="(item,index) in songInfoList" :key="index" @click="player" :data-mid="item.mid" :data-name="item.name" :data-singer="item.singer">
                <div>
                    <p>{{index+1}}.</p>
                </div>
                <view>
                    <p class="title">{{item.name}}</p>
                    <text v-for="(value,count) in item.singer" :key="count">{{value.name}}</text>
                </view>
            </div>
        </div>
    </div>
</template>
<script>
export default {
data(){
    return{
        data:{},
        songInfoList:[],
        num:0

    }
},
methods:{
    player(e){
        var arr = e.currentTarget.dataset.singer.map((item,index)=>{
            return item.name
        })
        var singer = arr.join('');
         wx.navigateTo({ url: '/pages/player/main?mid='+e.currentTarget.dataset.mid+'&name='+e.currentTarget.dataset.name+'&singer='+singer });
    },
},
onLoad(options){
    wx.showLoading({
      title: 'Loading...'
    });
    wx.request({
      url: 'https://u.y.qq.com/cgi-bin/musicu.fcg?-=getUCGI6688677494328337&g_tk=1789291141&loginUin=&hostUin=0&format=json&inCharset=utf8&outCharset=utf-8&notice=0&platform=yqq.json&needNewCode=0&data={%22detail%22:{%22module%22:%22musicToplist.ToplistInfoServer%22,%22method%22:%22GetDetail%22,%22param%22:{%22topId%22:'+options.id+',%22offset%22:0,%22num%22:20,%22period%22:%22%22}},%22comm%22:{%22ct%22:24,%22cv%22:0}}', //开发者服务器接口地址",
      data: {}, //请求的参数",
      method: 'GET',
      dataType: 'json', //如果设为json，会尝试对返回的数据做一次 JSON.parse
      success: res => {
          this.data=res.data.detail.data.data;
          this.songInfoList=res.data.detail.data.songInfoList
          res.data.detail.data.songInfoList.forEach((item,index)=>{
              this.num=index+1;
          })
      },
      fail: () => {},
      complete: () => {
          wx.hideLoading();
      }
    });
}    
}
</script>
<style scoped>
.box{
    width:100%;
    height:500rpx;
    flex-direction: column;
    text-align: center;
}
.box img{
    width:300rpx;
    height: 300rpx;
    border-radius: 20rpx;
    margin-top:30rpx;
}
.song{
    width:100%;
    height:100rpx;
    display: flex;
    margin-bottom: 30rpx;
    background: #30c27c;
    color:#fff;
}
.title{
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
}
.sort{
    width:100%;
    margin-bottom: 20rpx;
    color:brown;
}
</style>
