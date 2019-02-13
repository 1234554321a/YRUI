<template>
    <div id="letter">
        <div class="letter_content" v-for="(v,index) in list" :key="index" :id="'anchor-'+index">
            <div class="letter_title" v-if="v.personnel.length">{{v.letter}}</div>
            <div class="letter_list" v-for="(val,index) in v.personnel" :key="index">
                <img :src="val.avatar" alt="" srcset="">
                <span>{{val.name}}</span>
            </div>           
        </div>
        <div class="letter_index">
            <a v-for="(v,index) in list" :key="index" href="javascript:void(0)" @click="goAnchor('#anchor-'+index)" v-if="v.personnel.length">
                {{v.letter}}
            </a>
        </div>
    </div>
</template>
<script>
import {scrollTop, getScrollview} from '../../../utils/assist';
export default {
    props:{
        list:Array
    },
    name:'yd-letter',
    mounted(){
        this.scrollView = getScrollview(this.$el);
    },
    methods: {
        goAnchor(selector) {
            let anchor = this.$el.querySelector(selector)
            let top = 0;
            console.log(this.scrollView)
            if (this.scrollView === window) {
                if (document.documentElement && document.documentElement.scrollTop) {
                    top = document.documentElement.scrollTop;
                } else {
                    top = document.body.scrollTop;
                }
            } else {
                top = this.scrollView.scrollTop
            }
            scrollTop(this.scrollView, top, anchor.offsetTop);
        }
    }  
}
</script>
<style scoped>
    .letter_content{
        position: relative;
        z-index: 99;
        padding-left: .24rem;
        background-color: #fff;
    }
    .letter_title{
        background-color: rgba(242, 242, 242, 1);
        height: .48rem;
        font-size: .28rem;
        padding-left: .24rem;
        color: rgba(0, 0, 0, 0.54);
        line-height: .48rem;
        font-weight: bold;
        margin-left: -.24rem;
    }
    .letter_list{
        background-color: #fff;
        padding: .08rem 0;
        display: flex;
        font-size: .32rem;
        border-bottom: 1px solid rgba(230, 230, 230, 1);
        line-height: .8rem;
    }
    .letter_index{
        position: fixed;
        right: 0;
        top: 1rem;
        z-index: 100;
    }
    .letter_index a{
        display: block;
        color: rgba(144, 146, 165, 1);
        font-size: .22rem;
        height: .36rem;
        width: .36rem;
        text-align: center;
        line-height: .36rem;
    }
    .letter_list img{
        margin-right: .24rem;
        width: .8rem;
        height: .8rem;
        border-radius: 100px;
    }
</style>
