<style lang="scss" scoped>
    .box {
        width: 500px;
        padding: 10px;
        background-color: #fff;
        border-radius: 10px;
        box-shadow: 0 0 4px #000;

        ul {
            display: flex;

            li {
                flex: 1;
                text-align: center;
            }

            .active {
                color: #3bd415;
            }
        }

        .input {
            margin-top: 10px;

            span {
                font-size: 18px;
            }

            input {
                width: 80%;
                height: 30px;
                padding-left: 10px;
            }
        }

        .button {
            margin: 5px auto;

            button {
                width: 80px;
                height: 35px;
            }
        }

        .list {
            ul {
                display: flex;
                flex-direction: column;
                align-items: flex-start;
            }

            button {
                width: 80px;
                height: 35px;
            }
        }
    }
</style>
<template>
    <div class="content">
        <div class="box">
            <ul>
                <li @click="one(index = 1)" :class="{active: oneIndex == 1 || oneIndex == 2 || oneIndex == 3 || oneIndex == 4}">第一步</li>
                <li @click="one(index = 2)" :class="{active: oneIndex == 2 || oneIndex == 3 || oneIndex == 4}">第二步</li>
                <li @click="one(index = 3)" :class="{active: oneIndex == 3 || oneIndex == 4}">第三步</li>
                <li @click="one(index = 4)" :class="{active: oneIndex == 4}">最后一步</li>
            </ul>
            <div class="input">
                <span>库存量:</span><input type="text" v-model="inputValue">
            </div>
            <div class="button">
                <button v-if="oneIndex == 2 || oneIndex == 3 || oneIndex == 4">上一步</button>
                <button v-if="oneIndex == 4">提交</button>
                <button v-if="oneIndex == 1 || oneIndex == 2 || oneIndex == 3">下一步</button>
            </div>
            <div class="list">
                <ul>
                    <li>商品名称: 手机</li>
                    <li>商品价格: 333</li>
                    <li>商品图片: 1.jpg</li>
                    <li>商品库存: 333</li>
                </ul>
                <button>清空重填</button>
            </div>
        </div>
    </div>
</template>
<script>
    export default {
        data() {
            return {
                oneIndex: 1,
                inputValue: '',
            }
        },
        methods: {
            one(index) {
                if (this.inputValue == "" && this.oneIndex == 1) {
                    alert('内容不能为空')
                    return false
                }
                this.inputValue = ''
                this.oneIndex = index
            }
        },
        mounted() {
            document.body.style.backgroundColor = "#eee";
        },
    }
</script>