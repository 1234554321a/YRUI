<template>
    <div class="left-delete">
        <div class="move"
                @touchstart="_touchStart"
                @touchmove="_touchMove"
                @touchend="_touchEnd"
                :style="txtStyle">
                <!-- slot=父级内容 -->
            <slot></slot>
        </div> 
        <div :class="operating" ref='operating'>
            <div class="deleteIcon" :class="operating1.className" v-if="operating1" @click.prevent="deleteItem">{{operating1.name}}</div>    
            <div class="deleteIcon" :class="operating2.className" v-if="operating2" @click.prevent="view">{{operating2.name}}</div> 
            <div class="deleteIcon" :class="operating3.className" v-if="operating3" @click.prevent="collection">{{operating3.name}}</div>               
        </div>          
    </div>
</template>
<script>
export default {
    props:{
        operating1:Object,
        operating2:Object,
        operating3:Object,
        direction:{
            type: Boolean,
            default: 'right'
        },
    },
    name:"yd-skid",
    data() {
        return {
            startX: 0,       //触摸位置
            moveX: 0,       //滑动时的位置
            disX: 0,       //移动距离
            txtStyle: '',
            delWidth: 200,
            top: '',
            operating:''
        }
    }, 
    mounted(){
        this.operating=this.direction=='left'?'operatingLeft':'operatingRight'
    },
    methods:{
        _touchStart: function(ev) {
            ev = ev || event;
            if(ev.touches.length == 1){
                // 手指按下的时候记录按下的位置
                this.startX = ev.touches[0].clientX;
                // console.log(this.startX)
            }
        }, 
        _touchMove: function(ev) {
            ev = ev || event;
            if(ev.touches.length == 1) {
                // 滑动过程中的实时位置
                this.moveX = ev.touches[0].clientX
                // 滑动过程中实时计算滑动距离
                this.disX = this.startX - this.moveX;
                // console.log('disX==>',this.disX)
                // 如果是向右滑动或者只是点击，不改变滑动位置
                if(((this.direction=='right' && this.disX < 0) || this.disX == 0) || (this.direction=='left' && this.disX > 0)) {
                    // console.log('没有移动');
                    this.txtStyle = "transform:translateX(0px)";
                }else if (this.disX > 0 && this.direction=='right') {
                    // 如果是向左滑动，则实时给这个根元素一个向左的偏移-left，当偏移量到达固定值delWidth时，固定元素的偏移量为 delWidth
                    this.txtStyle = "transform:translateX(-"+this.$refs.operating.offsetWidth+"px)";
                    if (this.disX >= this.delWidth/100) {
                        this.txtStyle = "transform:translateX(-"+this.$refs.operating.offsetWidth+"px)";
                        this.zIndex = "z-index:" + 10 + "px";
                    }
                }else{
                    // 如果是向左滑动，则实时给这个根元素一个向左的偏移-left，当偏移量到达固定值delWidth时，固定元素的偏移量为 delWidth
                    this.txtStyle = "transform:translateX("+this.$refs.operating.offsetWidth+"px)";
                    if (this.disX >= this.delWidth/100) {
                        this.txtStyle = "transform:translateX("+this.$refs.operating.offsetWidth+"px)";
                        this.zIndex = "z-index:" + 10 + "px";
                    }                
                }
            }
        }, 
        _touchEnd: function(ev) {
            if (event.changedTouches.length == 1) {
                this.startX = 0;
                this.zIndex = "z-index:" + -1 + "px";
                // console.log(event.changedTouches[0].clientX)
                // 手指移动结束后的水平位置
                let endX = event.changedTouches[0].clientX;
                // 触摸开始与结束,手指移动的距离
                this.disX = this.startX - endX;
                //如果距离小于删除按钮的1/2，不显示删除按钮
            }
        }, 
        deleteItem: function(index) {
            this.$emit('deleteItem');
        },
        view(){
          this.$emit('view');  
        },
        collection(){
          this.$emit('collection');  
        }                
    }  
}
</script>
<style scoped>
    .left-delete{
        width:100%;
        height:100%;
        position:relative;
        z-index:2;      
    }
    .move{
        position: relative;
        transition: all .5s, ease .5s;
        -moz-transition: all .5s, ease .5s;
        -webkit-transition: all .5s, ease .5s;
        -o-transition: all .5s, ease .5s;  
    }
    .operatingLeft{
        position: absolute;
        left:0;
        top:0;
        display: flex;
        z-index: -1;
    }
    .operatingRight{
        position: absolute;
        right:0;
        top:0;
        display: flex;
        z-index: -1;
    }   
    .deleteIcon{
        color: #fff;
        font-size: .3rem;
        text-align: center;
        line-height: 1.48rem;
    }
</style>

