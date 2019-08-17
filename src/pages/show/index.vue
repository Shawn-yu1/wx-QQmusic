<template>
    <div>
            <div class="box" for="">
                <input type="text" placeholder="搜索歌曲、歌单、专辑" :value="msg" @input="changeInput" @change="setStorage"  @focus="foucs">
                <icon class="icon" type="search"></icon>
                <icon class="cancel" @click="cleanAll" type="cancel" v-if="cancel" color="#ccc"></icon>
                <text @click="clearAll" v-if="clear">取消</text>
            </div>
            <div class="songlist">
                <div class="song" v-for="(item,index) in list" :key="index" @click="player" :data-mid="item.mid" :data-name="item.name" :data-singer="item.singer">
                    <p class="musicName">{{item.name}}</p>
                    <text class="singer">{{item.singer}}</text>
                </div>
            </div>
            <div class="history" v-if="sex">
                <text>历史记录:</text>
                <div class="historyBox">
                    <div v-for="(item,index) in history" :key="index" @click="select(item)">
                        {{item}}
                    </div>
                </div>
            </div>
    </div>
</template>
<script>
export default {
    data(){
        return{
           list:[],
           history:[],
           msg:"",
           clear:false,
           cancel:false,
           sex:true
        }
    },
    methods:{
        cleanAll(){
            this.msg='';
            this.sex=true;
            this.cancel=false;
            this.clear=false;
        },
        clearAll(){
            this.msg='';
            this.clear=false;
            this.cancel=false;
            this.sex=true;

        },
        foucs(){
            this.clear=true;
            this.sex=false;
            if(this.msg){
                this.cancel=true;
            }
        },
        select(e){
            this.msg=e;
            this.searcherData(this.msg)
        },
        changeInput(e){
            console.log("123213213")
            if(e.mp.detail.value){
                this.cancel=true;
            }else{
                this.cancel=false;
                this.clear=false;
            }
            var name =e.mp.detail.value;
            this.msg=name;
            this.searcherData(this.msg)
        },
        searcherData(e){
             if(e){
                wx.request({
                url: "https://c.y.qq.com/splcloud/fcgi-bin/smartbox_new.fcg?is_xml=%200&format=jsonp&key="+e+"&g_tk=5381&loginUin=0&hostUin=0&format=jsonp&inCharset=utf-8&outCharset=utf-8&notice=0&platform=yqq&needNewCode=0", 
                data: 'data', //请求的参数",
                method: 'GET',
                dataType: 'json', //如果设为json，会尝试对返回的数据做一次 JSON.parse
                success: res => {
                    this.list=res.data.data.song.itemlist;
                },
                });            
                }
        },
        setStorage(e){
            var name = e.mp.detail.value;
             var history = wx.getStorageSync('history')||[];
            var what=history.indexOf(name);
            if(what==-1&&name!=''){
                history.unshift(name)
                if(history.length>=8){
                    history.pop()
                }
            }
            wx.setStorageSync('history',history);
            this.history=history;
        },
        player(e){
            wx.navigateTo({ url: '/pages/player/main?mid='+e.currentTarget.dataset.mid+'&name='+e.currentTarget.dataset.name+'&singer='+e.currentTarget.dataset.singer });
        }

    },
    created(){
        var history = wx.getStorageSync('history')||[];
        this.history=history;
    }


}
</script>

<style scoped>
.box{
    width: 100%;
    height:80rpx;
    background: gray;
    position: relative;
}
.box input{
    background: #fff;
    border-radius: 30rpx;
    width:80%;
    height:80%;
    position: absolute;
    left:20rpx;
    right: 0;
    top:10rpx;
    bottom:0;
    padding-left: 80rpx;
    box-sizing: border-box;
}
.box .icon{
    position: absolute;
    top:20rpx;
    left:50rpx;
}
.box .cancel{
    position: absolute;
    top:20rpx;
    right:150rpx;
    z-index: 100;
}
.box text{
    position: absolute;
    right:30rpx;
    top:10rpx;
}
.song{
    height:100rpx;
    border-bottom: 1px solid #ccc;
    width:100%;
}
.musicName{
    font-size: 30rpx;
}
.singer{
    font-size: 20rpx;
    color:grey;
}
.history{
    width:100%;
}
.history text{
    font-size: 40rpx;
    color:#ccc;
}
.historyBox{
    width:100%;
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
}
.historyBox div{
    font-size: 30rpx;
    width:200rpx;
    height:100rpx;
    border:1px solid black;
    text-align: center;
    line-height: 100rpx;
    margin-top:20rpx;
    border-radius: 30rpx;
    color:gray;
    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-line-clamp: 1;
    -webkit-box-orient: vertical;
    word-break: break-word;
}
</style>
