<style lang="scss" scoped>
    .table {
        width: 1000px;
        padding: 10px;
        background-color: #fff;
        margin: 100px auto;
        box-sizing: 0 0 3px #000;
        border-radius: 10px;

        table {
            width: 100%;

            .thead {
                background-color: #ebebeb;
                height: 35px;
                line-height: 35px;

                tr {
                    width: 100%;
                    display: flex;

                    th {
                        flex: 1;
                    }
                }
            }

            .tbody {
                background-color: #fff;

                tr {
                    width: 100%;
                    display: flex;

                    td {
                        flex: 1;
                        border: 1px solid #ccc;
                        height: 140px;
                        text-align: center;
                        font-size: 20px;
                        font-weight: 600;
                    }

                    td:hover {
                        background-color: rgba(0, 128, 255, 0.432);
                        color: #fff;
                    }
                }
            }
        }
    }

    .popup {
        position: absolute;
        width: 100%;
        height: 100vh;
        display: inline-block;
        background-color: rgba(0, 0, 0, 0.377);
        z-index: 8;
        top: 0;

        .v-enter-active,
        .v-leave-active {
            transition: opacity 1s ease;
        }

        .v-enter-from,
        .v-leave-to {
            opacity: 0;
        }

        .popup1 {
            width: 1000px;
            background-color: #fff;
            height: 600px;
            display: inline-block;
            position: absolute;
            left: 50%;
            margin-left: -500px;
            top: 50%;
            margin-top: -300px;
            padding: 20px;
            box-sizing: border-box;
            border-radius: 10px;
            box-shadow: 0 0 5px #000;
            overflow: hidden;

            .footer {
                width: 100%;
                display: flex;
                align-items: center;

                input {
                    width: 100%;
                    height: 36px;
                    padding-left: 10px;
                }
            }

            .box {
                ul {
                    li {
                        display: flex;
                        justify-content: space-between;
                        align-items: center;
                        border-bottom: 1px solid #ccc;
                    }
                }
            }
        }
    }
</style>
<template>
    <div class="content">
        <div class="table">
            <table>
                <div class="thead">
                    <tr>
                        <th>周日</th>
                        <th>周一</th>
                        <th>周二</th>
                        <th>周三</th>
                        <th>周四</th>
                        <th>周五</th>
                        <th>周六</th>
                    </tr>
                </div>
                <div class="tbody">
                    <tr>
                        <td @click="reveal('日')">
                            <ul>
                                <li v-for="(item, i) in list" :key="i" v-show="item.data == '日'">
                                    <div v-show="item.flag == false">{{item.value}}</div>
                                </li>
                            </ul>
                        </td>
                        <td @click="reveal('一')">
                            <ul>
                                <li v-for="(item, i) in list" :key="i" v-show="item.data == '一'">{{item.value}}</li>
                            </ul>
                        </td>
                        <td @click="reveal('二')">
                            <ul>
                                <li v-for="(item, i) in list" :key="i" v-show="item.data == '二'">{{item.value}}</li>
                            </ul>
                        </td>
                        <td @click="reveal('三')">
                            <ul>
                                <li v-for="(item, i) in list" :key="i" v-show="item.data == '三'">{{item.value}}</li>
                            </ul>
                        </td>
                        <td @click="reveal('四')">
                            <ul>
                                <li v-for="(item, i) in list" :key="i" v-show="item.data == '四'">{{item.value}}</li>
                            </ul>
                        </td>
                        <td @click="reveal('五')">
                            <ul>
                                <li v-for="(item, i) in list" :key="i" v-show="item.data == '五'">{{item.value}}</li>
                            </ul>
                        </td>
                        <td @click="reveal('六')">
                            <ul>
                                <li v-for="(item, i) in list" :key="i" v-show="item.data == '六'">{{item.value}}</li>
                            </ul>
                        </td>
                    </tr>
                </div>
            </table>
        </div>
        <transition>
            <div class="popup" v-show="show">
                <div class="popup1">
                    <div class="header">
                        <el-button type="warning" plain @click="close">关 闭</el-button>
                        <h2>周{{dates}}任务列表</h2>
                    </div>
                    <div class="footer">
                        <input type="text" v-model="inputValue">
                        <el-button type="primary" plain @click="add">添 加</el-button>
                    </div>
                    <div class="box">
                        <ul>
                            <li v-for="(item, i) in list" :key="i" v-show="item.data == dates">
                                <div>{{item.value}}</div>
                                <div>
                                    <el-button type="success" plain v-if="item.flag == false" @click="unfinish(item.flag, i)">未完成</el-button>
                                    <el-button plain v-if="item.flag == true" @click="unfinish(item.flag, i)">已完成</el-button>
                                    <el-button type="danger" plain @click="del(i)">删除</el-button>
                                </div>
                            </li>
                        </ul>
                    </div>
                </div>
            </div>
        </transition>
    </div>
</template>
<script>
    export default {
        data() {
            return {
                show: false,
                dates: '',
                inputValue: '',
                list: JSON.parse(localStorage.getItem('list')) || [],
            }
        },
        methods: {
            reveal(val) {
                console.log(val);
                this.dates = val
                this.show = true;
            },
            close() {
                this.show = false;
            },
            add() {
                if (this.inputValue == '') {
                    this.$message.error('内容不能为空');
                    return false
                }
                let obj = {
                    data: this.dates,
                    value: this.inputValue,
                    flag: false,
                }
                this.list.push(obj);
                this.$message({
                    message: '添加成功',
                    type: 'success'
                });
                localStorage.setItem('list', JSON.stringify(this.list));
                this.inputValue = ''
            },
            del(i) {
                this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
                    confirmButtonText: '确定',
                    cancelButtonText: '取消',
                    type: 'warning'
                }).then(() => {
                    this.list.splice(i, 1)
                    this.$message({
                        type: 'success',
                        message: '删除成功!'
                    });
                }).catch(() => {
                    this.$message({
                        type: 'info',
                        message: '已取消删除'
                    });
                });
            },
            unfinish(flag, i) {
                this.list[i].flag = !this.list[i].flag;
                localStorage.setItem('list', JSON.stringify(this.list));
                this.$message({
                    message: '已完成',
                    type: 'success'
                });
            }
        },
        mounted() {
            document.body.style.backgroundColor = "#eee";
        },
    }
</script>