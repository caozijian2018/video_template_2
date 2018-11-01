<template>
    <div class="container display_flex flex_column" :style="{height:height_}">
        <video-head @showLogin="showLogin" class="" :class="{height_scale:is_leave_top}"></video-head>
        <div @scroll="scroll" class="flex_1 overflow_scroll scroll_box" style="">
            <login-box v-if="show_login" @close="show_login=false"></login-box>
            <nuxt-child :is-scroll-bottom="is_leave_top"></nuxt-child>
        </div>
    </div>
</template>

<script>
    import videoHead from "../../components/videoHead"
    import loginBox from "../../components/loginBox"
    import initLanguage from "../../util/init_language";
    import initOp from "../../util/init_op";
    // import isScrollBottom from "../../util/is_scroll_bottom";
    import isScrollTop from "../../util/is_scroll_top";

    
    export default {
        components: {
            videoHead,
            loginBox
        },
        data() {
            return {
                prev_scroll: 0,
                height_: "",
                show_login: false,
                is_scroll_bottom: false,
                is_leave_top:false,
            }
        },
        mounted() {
            this.saveLang();
            this.saveOp();
            this.setHeight();
            this.watchOnresize();
        },
        methods: {
            scroll(res) {
                    var is_leave_top = isScrollTop(res);
                    // var after = res.target.scrollTop ;
                    console.log(is_leave_top);
                    this.is_leave_top = is_leave_top;
                    // console.log(after);
                    // var is_scroll_bottom = isScrollBottom(res, this);
                    // this.is_scroll_bottom = is_scroll_bottom;
            },
            saveLang() {
                var lang = this.$route.query.lang || this.$route.query.country;
                initLanguage(lang)
            },
            saveOp() {
                var op = this.$route.query.op ||  this.$route.query.country;
                initOp(op)
            },
            showLogin() {
                this.show_login = true;
            },
            watchOnresize() {
                window.onresize = this.setHeight;
            },
            setHeight() {
                this.height_ = innerHeight + "px";
            }
        }
    }
</script>

<style lang='less'>
    
</style>