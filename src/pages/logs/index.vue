<template>
  <div class="wrapper">
    <block v-for="(item,index) in msg" :key="index">
    <div class="box" @click="detailEvent" :data-id="item.id" :data-title="item.topTitle">
      <img :src="item.picUrl">
      <div class="right">
        <p>{{item.topTitle}}</p>
        <div class="songlist">
          <block v-for="(item2,index2) in item.songList" :key="index2">
            <view class="title"><em>{{index2+1}}</em> {{item2.songname}}-<text>{{item2.singername}}</text></view>
          </block>
          <div class="point"></div>
        </div>
      </div>
    </div>
    </block>
  </div>
</template>

<script>
export default {
  data () {
    return {
      msg:[]
    
    }
  },
  methods:{
    detailEvent(e){
      wx.navigateTo({ url: '/pages/detail/main?id='+e.currentTarget.dataset.id+'&title='+e.currentTarget.dataset.title });
    }
  },

  created(){
    wx.showLoading({
      title: 'Loading...', //提示的内容,
    });
    wx.request({
      url: 'https://c.y.qq.com/v8/fcg-bin/fcg_myqq_toplist.fcg?_=1554704775937&g_tk=5381&uin=0&format=json&inCharset=utf-8&outCharset=utf-8&notice=0&platform=h5&needNewCode=1', //开发者服务器接口地址",
      data: {}, //请求的参数",
      method: 'GET',
      dataType: 'json', //如果设为json，会尝试对返回的数据做一次 JSON.parse
      success: res => {
        let data = res.data.data.topList;
        this.msg=data;
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
.wrapper{
  background: #F0F0F0;
  overflow: hidden;
}
.box{
  width:100%;
  height:200rpx;
  display: flex;
  justify-content: space-between;
  background: #fff;
  margin-top:50rpx;
}
.box img{
  width:30%;
  height:200rpx;
}
.right{
  width:65%;
  flex-direction: column;

}
.title{
    display: flex;
    font-size: 30rpx;
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
}
.songlist{
  margin-top:20rpx;
  position: relative;
}
.title text{
  color:#ccc;
}
.title em{
  margin-right:10rpx;
  color: gray;
}
.point{
  width:10rpx;
  height:10rpx;
  border-right:1px solid black;
  border-bottom: 1px solid black;
  transform: rotate(-45deg);
  position: absolute;
  right:20rpx;
  top:30rpx;
}
</style>

