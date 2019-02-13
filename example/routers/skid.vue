<template>
    <yd-layout title="Skid">
        <div>
            <yd-skid :operating1="operating1" direction="left">
                <div class="skid_center">向左滑动</div>
            </yd-skid>
            <yd-skid :operating1="operating1" :operating2='operating2'>
                <div class="skid_center">向右滑动</div>
            </yd-skid>           
        </div>
    </yd-layout>
</template>
<script>
export default {
    data(){
        return{
            operating1:{className:'height bjcolor1',name:'删除'},
            operating2:{className:'height bjcolor2',name:'删除'},
            operating3:{className:'height bjcolor3',name:'删除'},
        }
    },
}
</script>
<style>
.skid_center{
    height: 1.48rem;
    background-color: #fff;
}
.bjcolor1{
    background-color: rgba(0, 183, 238, 1)
}
.bjcolor2{
    background-color: rgba(239, 106, 106, 1)
}
.bjcolor3{
    background-color: rgba(145, 219, 96, 1)
}
.height{
    width: 1.48rem;
}
</style>
