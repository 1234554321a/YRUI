<template>
    <div id="up_img">
        <div class="up_img" @click="_up">
            <slot></slot>
        </div>
        <div class="img_list" v-for="(v,index) in img_list" :key="index" :style="{width:width,height:height}">
            <span @click="_remove(index)">
                x
                <!-- <img src='../upimage/icon-delete.png' alt="" srcset=""> -->
            </span>
            <span>
                <img :src="v" alt="" srcset="">
            </span>
        </div>
    </div>
</template>
<script>
export default {
    name:'yd-upimage',
    props:{
        data:Object,
        url:String,
        height:String,
        width:String
    },
    data(){
        return{
            img_list:[],//图片列表
            index:0,
            localId_list:[]
        }
    },
    methods:{
        _remove(index){
           this.img_list.splice(index,1) 
        },
        _up(){
            let _that=this
            wx.chooseImage({
                count: 9, // 默认9
                sizeType: ['original', 'compressed'], // 可以指定是原图还是压缩图，默认二者都有
                sourceType: ['album', 'camera'], // 可以指定来源是相册还是相机，默认二者都有
                success: function (res) {
                    let localIds = res.localIds; // 返回选定照片的本地ID列表，localId可以作为img标签的src属性显示图片
                    _that.localId_list=localIds
                    _that._down(localIds[_that.index])
                }
            });          
        },
        _down(localIds){
            let _that=this
            wx.uploadImage({
                localId: localIds, // 需要上传的图片的本地ID，由chooseImage接口获得
                isShowProgressTips: 1, // 默认为1，显示进度提示
                success: function (res) {
                    let serverId = res.serverId; // 返回图片的服务器端ID
                    if(_that.localId_list.length>_that.index){
                        this.data.server_id=serverId
                        axios({
                            method:'POST',
                            url:this.url,
                            data:this.data,
                        })
                        .then(function (res) {
                            _that.img_list.push(res.data.data)
                            _that.$emit('imgList',_that.img_list)
                        })
                        .catch(function (err) {
                            console.log(err.response)
                        })                      
                        ++_that.index
                        _that._down(_that.localId_list[_that.index]) 
                    }
                }
            });          
        }
    }

}
</script>
<style scoped>
    #up_img{
        display: flex;
        flex-wrap: wrap;
    }
    .img_list{
        position: relative;
        background-color: #fff;
        width:1.6rem;
        height:1.6rem;
        margin-left:.24rem;
        margin-top: .24rem; 
    }
    .img_list>span:first-child{
        position: absolute;
        top: 0;
        right: 0;
        width: .31rem;
        height: .32rem;
        background-color: rgba(239, 106, 106, 1);
        color: #fff;
        border-radius: .32rem;
        text-align: center;
        line-height: .3rem;
        font-size: 12px;
    }
    img{
        width: 100%;
        height: 100%;
    }
</style>
