<template>
    <div class="bigbox">
        <div class="head_box width_80 white margin_auto height_100 display_flex flex_jusify_space flex_align_end">
            <div class="display_flex flex_align_end height_100 logo_box">
                <nuxt-link :to='{path:"/"}'>
                    <img src="../../static/img/logo.png" class="logo_img" alt="">
                </nuxt-link>
                <div class="display_flex flex_align_end   classic_box">
                    <div class="categaray">
                        <nuxt-link :to="{path:'/categaray'}" class="white">
                            categaray
                        </nuxt-link>
                    </div>
                </div>
            </div>
            <div class="member_login_2_search_box width_25 display_flex flex_column">
                <div class="margin_bottom_1 search_box display_flex flex_jusify_space" :class="{width_100px:leave_bottom_100width}">
                    <input @keyup.13="Search" v-model="search_word" type="text" class="input_search flex_1">
                    <div class="search_icon_button iconfont  icon-fangdajing" @click="searchOrlongInput">
                    </div>
                </div>
                <div class="display_flex flex_jusify_space flex_align_center">
                    <div class="font_size_8 login_text">
                        Member Login
                    </div>
                    <login-button :text-button="$t('words.login')" @click.native="showLogin()"></login-button>
                </div>
            </div>
            <div class="display_flex flex_align_center phone_show">
                <login-button :text-button="$t('words.login')" @click.native="showLogin()"></login-button>
            </div>
        </div>
        <div class="phone_show phone_search_box ">
            <div class="width_95 margin_auto height_100 display_flex flex_jusify_space flex_align_center">
                <div class="option_animate display_flex overflow_hidden flex_align_center" :class="{selected_op:show_option_var}" @click.stop="show_option">
                    <div class="display_flex line_box white flex_jusify_space flex_column">
                        <div class="first"></div>
                        <div class="secend"></div>
                        <div class="last"></div>
                    </div>
                </div>
                <div class="flex_1 margin_left_1 position_relative">
                    <input type="text" @keyup.13="Search" v-model="search_word" class="search_phone_input">
                    <i class="iconfont phone_fangdaj icon-fangdajing" @click="Search"></i>
                </div>
            </div>
        </div>
    
        <div class="hoverdiv cate_hover hide_cate" :class="{show_out_cate:show_option_var}" style="top:110px">
            <div class="categoray_box white position_absolute">
                <div  >
                    <nuxt-link @click.native="show_option_var=false" class="white font_size_4" :to="{path:'/categaray'}">{{$t('words.categories')}}</nuxt-link>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import videoButton from "../button";
    import loginButton from "../login_button";
    
    import getCountry from "../../util/get_country";
    import bus from "../../util/bus";
    import unlogin from "../../util/unlogin";
    import getOp from "../../util/get_country";
    
    export default {
        props: {
            isLeaveTop: {
                default: true,
                type: Boolean
            }
        },
        data() {
            return {
                leave_bottom_100width: false,
                search_word: "",
                clickSelect: "",
                selectOrder: "",
                showWhich: "",
                show_option_var: false,
                op: "",
                show_login_button: false,
                show_unlogin_button: false,
                arr: [{
                        name: this.$t("words.most_recent"),
                        id: "-create_time"
                    },
                    {
                        name: this.$t("words.most_liked"),
                        id: "-favor_num"
                    },
                    {
                        name: this.$t("words.most_viewed"),
                        id: "-show_num"
                    }
                ]
            };
        },
        components: {
            videoButton,
            loginButton
        },
        mounted() {
            this.getOp();
            this.watchBus();
        },
        methods: {
            show_option() {
                this.show_option_var = !this.show_option_var;
            },
            searchOrlongInput() {
                if (this.isLeaveTop) {
                    if (this.leave_bottom_100width) {
                        if (this.search_word) {
                            this.Search();
                        }
                    }
                    this.leave_bottom_100width = !this.leave_bottom_100width;
                } else {
                    this.Search();
                }
            },
            watchBus() {
                bus.$on("loginSuccess", () => {
                    this.diffrentOp();
                });
            },
            unLogin() {
                unlogin();
                this.diffrentOp();
                this.$msg(this.$t("words.unlogin_success"));
            },
            diffrentOp() {
                switch (this.op) {
                    case "tw":
                        (() => {
                            this.show_login_button = !localStorage.video_token;
                            this.show_unlogin_button = localStorage.video_token;
                        })();
                        break;
                    default:
                        (() => {
                            this.show_login_button = false;
                            this.show_unlogin_button = false;
                        })();
                        break;
                }
            },
            getOp() {
                this.$nextTick(() => {
                    this.op = getOp();
                    this.diffrentOp();
                });
            },
            Search() {
                this.show_option_var=false;
                bus.$emit("search", this.search_word);
                this.showWhich = "";
                this.$store.commit("search", this.search_word);
                this.search_word = "";
                this.$router.push({
                    path: "/"
                });
            },
            goHome() {
                this.$router.push({
                    path: "/"
                });
            },
            showLogin() {
                this.$emit("showLogin");
            },
            toCate() {
                this.showWhich = "";
                this.$router.push({
                    name: "index-index-categaray"
                });
            },
            orderselect(ordering) {
                this.clickSelect = "";
                this.selectOrder = ordering;
                // alert(ordering);
            },
            getList(ordering, pc_ph) {
                if (pc_ph == "pc") {
                    this.clickSelect = ordering;
                } else if (pc_ph == "ph") {
                    this.showWhich = "";
                }
                if (this.$route.name != "index-index") {
                    this.$router.push({
                        name: "index-index"
                    });
                }
                bus.$emit("ordering", ordering);
            },
            showWichbox(type, mouse) {
                if (mouse) {
                    if (screen.width < 800) {
                        return;
                    }
                    this.showWhich = "";
                } else {
                    this.showWhich = type == this.showWhich ? "" : type;
                }
            }
        }
    };
</script>

<style lang='less'>
    @import "../../assets/css/current_theme";
    @transition_time: 0.4s;
    .bigbox {
        .categoray_box{
            left:-70%;
        }
        .cate_hover {
            z-index: -111;
        }
        .login_text {
            min-width: 120px;
        }
        .member_login_2_search_box {
            transition: @transition_time;
        }
        .search_box {
            width: 100%;
            transition: @transition_time;
            height: 30px;
            .search_icon_button {
                width: 30px;
                height: 100%;
                background: rgba(96, 96, 96, 1.0);
                color: #fff;
                font-size: 20px;
                text-align: center;
                // vertical-align: middle;
                line-height: 30px;
            }
            .input_search {
                transition: @transition_time;
                height: 100%;
                background: #fff;
                min-width: 0;
                outline: none;
                border: none;
            }
        }
        .login_button {
            text-align: center;
            padding: 5px 10px;
            background: @app_green;
            color: #000;
            font-weight: 600;
        }
        .classic_box {
            &>div {
                cursor: pointer;
                margin: 0 10px;
                font-size: 20px;
                color: #fff;
                transition: @transition_time;
                &:hover {
                    color: rgba(118, 214, 195, 1.0);
                }
            }
        }
        overflow: hidden;
        height: 90px;
        &.height0 {
            height: 0px;
        }
        padding-bottom: 10px;
        padding-top: 10px;
        background: lighten( @app_head_back, 19%);
        transition-duration: @transition_time;
        transition-property: height;
        .head_box {
            .logo_img {
                height: 70px;
                transition: @transition_time;
            }
        }
        &.height_scale {
            .member_login_2_search_box {
                width: 30%;
                flex-direction: row-reverse;
                align-items: center;
                .search_box {
                    margin-left: 10px;
                    margin-bottom: 0;
                    width: 30px;
                    &.width_100px {
                        width: 170px;
                    }
                }
                .login_button {
                    margin-left: 10px;
                }
                .input_search {
                    // width: 0px;
                }
            }
            & {
                height: 50px;
            }
            .head_box {
                .logo_img {
                    height: 40px;
                }
            }
        }
        @media screen and (max-width: 800px) {
            & {
                .hide_cate{
                    z-index:-1;
                    // display: none;
                    
                    &>.categoray_box{
                        left:-70%;
                        transition: 0.3s;
                        transition-property: left;
                    }
                }
                .show_out_cate{
                    // display: block;
                    z-index:10004;
                    &>.categoray_box{
                        left:0;
                    }
                }
                .categoray_box {
                    z-index: 10003;
                    padding: 0 10px 30px;
                    width: 60%;
                    background: #333;
                    &>div {
                        padding: 10px;
                        width: 90%;
                        margin: auto;
                        border-bottom: 1px solid #fff;
                    }
                }
                .phone_fangdaj {
                    position: absolute;
                    right: 10px;
                    top: 4px;
                    color: #fff;
                    font-size: 20px;
                }
                .search_phone_input {
                    width: 100%;
                    box-sizing: border-box;
                    height: 30px;
                    background: #333;
                    outline: none;
                    border: none;
                    color: #fff;
                    font-size: 16px;
                    padding-right: 40px;
                    padding-left: 10px;
                }
                .head_box {
                    height: 50px;
                }
                height:90px;
                .line_box>div {
                    transition-duration: .4s;
                }
                &.height_scale {
                    height: 90px;
                }
                .option_animate {
                    padding: 0 10px;
                    height: 40px;
                    width: 35px;
                    &>div {
                        width: 100%;
                        height: 25px;
                        div {
                            width: 100%;
                            height: 3px;
                            border-radius: 1.5px;
                            background: #fff;
                        }
                    }
                }
                .selected_op {
                    .first {
                        transform-origin: 0 0;
                        transform: translateX(4px) rotate(39deg);
                    }
                    .secend {
                        opacity: 0;
                    }
                    .last {
                        transform-origin: 0 100%;
                        transform: translateX(4px) rotate(-39deg);
                    }
                }
                .phone_search_box {
                    height: 40px;
                    padding: 0 0 10px;
                }
                .member_login_2_search_box {
                    display: none;
                }
                .logo_box {
                    align-items: center;
                }
                // height: 40px;
                .classic_box {
                    display: none;
                }
                .head_box {
                    align-items: center;
                    width: 95%;
                    .logo_img {
                        height: 40px;
                    }
                }
            }
        }
    }
</style>