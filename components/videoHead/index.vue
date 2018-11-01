<template>
    <div class="bigbox">
        <div class="head_box width_80 white margin_auto height_100 display_flex flex_jusify_space flex_align_end">
            <div class="display_flex flex_align_end height_100">
                <img src="../../static/img/logo.png" class="logo_img" alt="">
                <div class="display_flex flex_align_end   classic_box">
                    <div class="categaray">
                        categaray
                    </div>
    
                </div>
            </div>
            <div class="member_login_2_search_box width_25 display_flex flex_column">
                <div class="margin_bottom_1 search_box display_flex flex_jusify_space">
                    <input type="text" class="input_search flex_1">
                    <div class="search_icon_button iconfont  icon-fangdajing">
                    </div>
                </div>
                <div class="display_flex flex_jusify_space flex_align_center">
                    <div class="font_size_8 login_text">
                        Member Login
                    </div>
                    <div class="login_button font_size_8 cursor" @click="showLogin()">
                        JOIN NOW
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import videoButton from "../button";
    import getCountry from "../../util/get_country";
    import bus from "../../util/bus";
    import unlogin from "../../util/unlogin";
    import getOp from "../../util/get_country";
    
    export default {
        // props:{
        //     height_scale:{
        //         default:true,
        //         type:Boolean
        //     }
        // },
        data() {
            return {
                search_word: "",
                clickSelect: "",
                selectOrder: "",
                showWhich: "",
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
            videoButton
        },
        mounted() {
            this.getOp();
            this.watchBus();
        },
        methods: {
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
                height: 50px;
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
                    height: 30px;
                }
            }
        }
        @media screen and (min-width: 800px) {}
    }
</style>