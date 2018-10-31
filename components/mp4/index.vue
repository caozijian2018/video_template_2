<template>
    <div class="">
        <video id="mp4" autoplay class="pcs width_100" controls="controls" preload  :src="mp4Src"></video>
        <div id="model_circle">
             18+
        </div>
    </div>
</template>

<script>
    export default {
        props:{
            mp4Src:{
                type:String
            }
        },
        data() {
            return {
               
            }
        },
        mounted() {
            this.controlTime();
        },
        methods: {
            controlTime(){
               var videoNode = document.getElementById("mp4")
                 videoNode.addEventListener("timeupdate", (res)=>{
                    // var play_time = res.target.currentTime
                    // var duration = res.target.duration;
                    // console.log(play_time);
                    // console.log(duration);

                    // if((duration-play_time >10 && play_time>30)){
                    // }else{
                    //     res.target.currentTime=31;
                    // }
                 });
            }

        }
    }
</script>

<style lang="less">
    #model_circle{
        width:150px;
        height:60px;
        color:#fff;
        background: rgba(216, 21, 102, 0.8);
        font-size:23px;
        border-radius: 5px;
        text-align: center;
        line-height: 60px;
        font-weight:800;
        position: absolute;
        right: 10px;
        z-index: 10000;
        bottom: -50px;

    
    }
    
</style>
