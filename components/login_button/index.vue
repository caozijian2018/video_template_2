<template>
    <div>
        <div class="login_button font_size_8 cursor">
            {{textButton}}
        </div>
    </div>
</template>

<script>
    export default {
        props:{
            textButton:{
                type:String,
                default:'HELLO'
            }
        }
    }
</script>

<style lang='less'>
    @import "../../assets/css/current_theme";
    .login_button {
            text-align: center;
            padding: 5px 10px;
            background: @app_green;
            color: #000;
            font-weight: 600;
        }
</style>