<template>
    <div class="detail">
            <div class="name">{{name}}</div>
            <div class="singer">{{singer}}</div>
            <img class="picture" :src="url" alt="">
            <div class="btn">
                <img v-if="flag" @click="audioPlay" src="../../../static/images/play.png" alt="">
                <img v-else @click="audioPause" src="../../../static/images/pause.png" alt="">
                <img @click="audio14" src="../../../static/images/seek.png" alt="">
                <img @click="audioStart" src="../../../static/images/return.png" alt="">
            </div>
            <div v-if="panduan" class="geci" @click="control">隐藏歌词</div>
            <div v-else class="geci" @click="control">显示歌词</div>
            <scroll-view scroll-y style="height: 200px;" v-if="panduan"  :scroll-top="top">
                <div class="songci">
                    <div v-for="(item,index) in newArr" :key="index" :class="item.key==nowTime?'font':'normol'" :style="{'color':item.key==nowTime?'#30c27c':'#000'}">
                        {{item.value}}
                    </div>
                </div>
            </scroll-view>
    </div>
</template>
<script>
var innerAudioContext = wx.createInnerAudioContext();
export default {
    data(){
        return{
           name:'',
           singer:'',
           newArr:[],
           nowTime:'',
           top:50,
           url:'',
           flag:true,
           panduan:true
        }
    },
    methods:{
        control(){
            this.panduan=!this.panduan;
        },
        audioPlay(){
            this.flag=false;
            innerAudioContext.play();
             innerAudioContext.onTimeUpdate(()=>{
               this.lyricPlay()
            })
        },
        audioPause(){
            this.flag=true;
            innerAudioContext.pause()
        },
        audio14(){
            innerAudioContext.seek(10)
        },
        audioStart(){
            innerAudioContext.seek(0);
            
        },
        lyricCombine(str){
             var newArr=[];
            var arr=str.split(/\n(s+)?/g);
            for(var i=0;i<arr.length;i++){
                if(arr[i]==undefined){
                    arr.splice(i,1)
                }
                var arr2=arr[i].split(']');
                var key = arr2[0].substr(1);
                key=key.substr(0,5);
                var value = arr2[1];
                var json ={
                    key:key,
                    value:value
                }
                newArr.push(json)
            }
            newArr.splice(0,5);
            for(var i=0;i<newArr.length;i++){
                if(newArr[i].value==''){
                    newArr.splice(i,1)
                }
            }
            this.newArr=newArr
        },
        lyricPlay(){
           let currentTime = innerAudioContext.currentTime;
           var minute = Math.floor(currentTime/60)<10?"0"+Math.floor(currentTime/60):Math.floor(currentTime/60);
           var second=Math.floor(currentTime%60)<10?"0"+Math.floor(currentTime%60):Math.floor(currentTime%60);
           var time = minute+":"+second;
           for(var i=0;i<this.newArr.length;i++){
               if(this.newArr[i].key==time){
                   this.top=(i-2)*20;
                   this.nowTime=time;
               }
           }
        }
    },
    onLoad(options){
            this.name=options.name;
            this.singer=options.singer;
            this.url="https://v1.itooi.cn/tencent/pic?id="+options.mid;
            innerAudioContext.src="https://v1.itooi.cn/tencent/url?id="+options.mid+"&quality=192";
            innerAudioContext.onPlay(()=>{
                console.log("开始播放")
            })
            innerAudioContext.onEnded(()=>{
                this.flag=true;
            })
            //请求歌词信息
            wx.request({
              url: 'https://v1.itooi.cn/tencent/lrc?id='+options.mid+'', //开发者服务器接口地址",
              success: res => {
                  this.lyricCombine(res.data);
                 
              },
            });
    }
}
</script>
<style scoped>
.detail{
    text-align: center;
}
croll-view{
    position: relative;
}
.songci{
    text-align: center;
    font-size: 30rpx;
    font-family: "微软雅黑";
}   
.box{
    width:100%;
    margin-top:50rpx;
}
.btn{
    width: 100%;
    height: 100rpx;
    display: flex;
    justify-content: space-around;
}
.btn img{
    width:100rpx;
    height:100rpx;
}
.picture{
    width: 500rpx;
    height:500rpx;
    margin:30rpx 0;
}
.geci{
    background:black;
    color:#fff;
    margin-top:20rpx;
    margin-bottom:20rpx;
    border-radius: 30rpx;
}
.songci div{
    margin:10rpx 0;
    text-shadow: 5px 5px 5px #ccc;
}
.name{
    font-size:50rpx;
    font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
    font-family: "微软雅黑";
}
.singer{
    font-size: 30rpx;
    color:#ccc;
}
.font{
    font-size: 40rpx;
    text-shadow: 5px 5px 5px #30c27c;
}


</style>
